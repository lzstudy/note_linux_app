alsa-lib编程
=============

1 基本介绍
-----------

一张声卡用card来表示, 每张声卡可以有多个设备, 每个设备可以同时支持播放和录音

1.1 参考例程
*************

===================== ======================================
alsa_play.tar.gz_     alsa播放例程, 内含测试音频
===================== ======================================

.. _alsa_play.tar.gz: http://120.48.82.24:9100/note_linux_app/alsa_play.tar.gz

1.2 编程流程
*************

#. 打开wav文件
#. 读取wav的chunk, 保存其中的参数
#. 打开音频设备
#. 设置硬件参数, 根据wav的参数设置(访问类型、数据格式、声道数、采样率、周期大小、缓冲区数)
#. 循环读写数据, 播放为写, 录音为读
#. 关闭设备

.. note:: 
    
    wav由chunk + pcm数据构成, 且是连续存储的, 所以先读取chunk数据获取相关参数, 在lseek到数据区
    将数据写入声卡


1.3 设备节点
*************

一张带播放和录音的设备至少有三个设备节点：控制节点, 播放设备, 录音设备.

``/dev/snd/controlC0``
    声卡0控制节点, 例如通道选择、混音器和麦克风控制, 例如C0表示声卡0(card0)。

``/dev/snd/pcmC0D0c``
    C0表示声卡0, D0表示设备0, c表示录音(capture)。

``/dev/snd/pcmC0D0p``
    C0表示声卡0, D0表示设备0, p表示播放设备。


1.4 头文件与链接库
******************

编译时指定sysroot路径即可

.. code-block:: c

    # 头文件
    #include <alsa/asoundlib.h>

    # 库名字
    libasound.so

2 代码编程
------------

2.1 wav的chunk结构
*********************

.. code-block:: c

    typedef struct _riff_chunk {
        char id[4];                                                                 /**@ id, 固定RIFF. */
        uint32_t size;                                                              /**@ wav文件大小. */
        char type[4];                                                               /**@ 类型. */
    }__attribute__((packed)) riff_chunk;

    typedef struct _format_chunk {
        char id[4];                                                                 /**@ 固定fmt. */
        uint32_t size;                                                              /**@ 固定16. */
        uint16_t format;                                                            /**@ 固定1. */
        uint16_t channel;                                                           /**@ mono = 1, stereo = 2. */
        uint32_t srate;                                                             /**@ 采样率 */
        uint32_t brate;                                                             /**@ 每秒字节数. */
        uint16_t align;                                                             /**@ 块对齐. */
        uint16_t bps;                                                               /**@ 采样位数. */
    }__attribute__ ((packed)) format_chunk;

    typedef struct data_chunk {
        char id[4];                                                                 /**@ 固定data. */
        uint32_t size;                                                              /**@ 大小. */
    }__attribute__ ((packed)) data_chunk;

    typedef struct _wav_chunk {
        riff_chunk riff;                                                            /**@ riff chunk. */
        format_chunk fmt;                                                           /**@ fmt chunk. */
        data_chunk data;                                                            /**@ data chunk. */
    }__attribute__ ((packed)) wav_chunk;

2.2 声卡配置
**************

.. code-block:: c

    /**************************************************************************************************
    * @brief  : 声卡配置
    * @param  : None
    * @return : None
    **************************************************************************************************/
    static int snd_card_config(snd_context *cxt)
    {
        int ret;
        snd_pcm_t *pcm;
        snd_pcm_hw_params_t *sphp;
        wav_chunk *chunk = &cxt->chunk;

        /* 打开设备 */
        ret = snd_pcm_open(&pcm, SND_PLAYBACK_DEV, SND_PCM_STREAM_PLAYBACK, 0);
        CHECK_RET(ret < 0, ret, "snd %s open fail(%d)", SND_PLAYBACK_DEV, ret);

        /* 创建SPHP */
        snd_pcm_hw_params_malloc(&sphp);

        /* 获取当前硬件配置 */
        ret = snd_pcm_hw_params_any(pcm, sphp);
        CHECK_GOTO(ret < 0, err_to_free_pcm, "get pcm param fail(%d)", ret);

        /* 设置访问类型：交错模式 */
        ret = snd_pcm_hw_params_set_access(pcm, sphp, SND_PCM_ACCESS_RW_INTERLEAVED);
        CHECK_GOTO(ret < 0, err_to_free_sphp, "snd set sphp fail(%d)", ret);

        /* 设置数据格式：有符号16位、小端模式 */
        ret = snd_pcm_hw_params_set_format(pcm, sphp, SND_PCM_FORMAT_S16_LE);
        CHECK_GOTO(ret < 0, err_to_free_sphp, "set format fail(%d)", ret);

        /* 设置采样率 */
        ret = snd_pcm_hw_params_set_rate(pcm, sphp, chunk->fmt.srate, 0);
        CHECK_GOTO(ret < 0, err_to_free_sphp, "set rate fail(%d)", ret);

        /* 设置通道 */
        ret = snd_pcm_hw_params_set_channels(pcm, sphp, chunk->fmt.channel);
        CHECK_GOTO(ret < 0, err_to_free_sphp, "set snd channel fail(%d)", ret);

        /* 设置周期大小 */
        ret = snd_pcm_hw_params_set_period_size(pcm, sphp, SND_PERIOD_SIZE, 0);
        CHECK_GOTO(ret < 0, err_to_free_sphp, "set set period fail(%d)", ret);

        /* 设置周期数 */
        ret = snd_pcm_hw_params_set_periods(pcm, sphp, SND_PERIODS, 0);
        CHECK_GOTO(ret < 0, err_to_free_sphp, "set set periods fail(%d)", ret);

        /* 使能配置 */
        ret = snd_pcm_hw_params(pcm, sphp);
        CHECK_GOTO(ret < 0, err_to_free_sphp, "enable pcm hw param fail(%d)", ret);

        /* 申请应用缓冲 */
        cxt->size = SND_PERIOD_SIZE * chunk->fmt.align;
        cxt->buf = malloc(cxt->size);
        CHECK_GOTO(cxt->buf == NULL, err_to_free_sphp, "snd malloc buf fail");

        /* 释放参数缓冲 */
        snd_pcm_hw_params_free(sphp);

        /* 保存参数 */
        cxt->pcm = pcm;
        LOG_I("snd init ok.");
        return 0;

    err_to_free_sphp:
        snd_pcm_hw_params_free(sphp);
    err_to_free_pcm:
        snd_pcm_close(pcm);
        return ret;
    }


