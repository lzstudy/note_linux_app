alsa-utils使用
===============

1 aplay
--------

aplay用于测试音频播放功能, 可以用aplay播放wav格式的音频文件,

.. code-block:: c

   # 播放音频, 采用默认声卡播放
   aplay test.wav

   # 使用指定声卡播放(例 使用声卡1设备0播放)
   # aplay -D hw:card,device test.wav
   aplay -D hw:1,0 test.wav

.. note:: 
   
   aplay只能播放wav格式的音频, mp3类的音频文件需要用其他工具, 例如ffmpeg或者gsteamer

2 arecord
----------

arecord用于录音

.. code-block:: c

   # 录制一个cd级的10秒的音频
   arecord -f cd -d 10 test.wav

.. note:: 
   
   - -f 指定音频格式, cd表示cd级别(16位小端,44100,sterro)
   - -d 指定录制的时间


3 alsamixer
------------

alsamixer用于配置声卡的混音器, 是一个图形化工具, 指令如下

======= =====================
指令     指令描述
======= =====================
h       帮助
F6      选择声卡
上      音量加(左右声道一起加)
下      音量减(左右声道一起减)
q       左声道音量加
z       左声道音量减
e       右声道音量加
c       右声道音量减
======= =====================

.. warning:: 
   
   使用alsamixer需要设置XTERM环境变量 ``export TERM=xterm-256color``

4 alsactl
-----------

alsamixer虽然可以配置声卡, 但是每次断点后配置会消失, 因此我们需要保存配置。

.. code-block:: c

   # 保存配置
   alsactl -f /va/lib/alsa/asound.state store

   # 加载配置
   alsactl -f /var/lib/alsa/asound.state restore

5 amixer
----------

amixer也是声卡配置工具, 与alsamixer功能相同, 只不过是基于字符配置的

.. code-block:: c

   # 配置耳机音量左右声道都为100
   amixer sset Headphone 100,100

