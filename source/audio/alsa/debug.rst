音频调试
=========

1 调试流程
-----------

2 调试指令
-----------

2.1 查看声卡数量
****************

.. code-block:: c

    # 方法1 通过proc属性查看
    $ cat /proc/asound/cards
    0 [imxaudioxcvr   ]: imx-audio-xcvr - imx-audio-xcvr
                         imx-audio-xcvr
    1 [btscoaudio     ]: bt-sco-audio - bt-sco-audio
                         bt-sco-audio
    2 [wm8960audio    ]: wm8960-audio - wm8960-audio
                         wm8960-audio
    3 [audiohdmi      ]: audio-hdmi - audio-hdmi
                         audio-hdmi

    # 方法2 通过proc目录查看
    $ ls /proc/asound/
    card0/ card1/ card2/ card3/

    # 方法3 查看/dev/snd节点, 观察control节点数量
    $ ls /dev/snd/
    controlC0 controlC1 controlC2 controlC3 ...

2.2 查看声卡device
*******************

.. code-block:: c

    # 其中x根据自己选择
    $ls /proc/asound/cardx
    id  pcm0c/  pcm0p/  pcm1c/  pcm1p/


