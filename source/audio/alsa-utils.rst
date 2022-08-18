alsa工具
========

混音器设置[alsamixer]
---------------------

声卡配置工具

.. note::

   - 上 音量加
   - 下 音量减
   - q 左声道音量加
   - z 左声道音量减
   - e 右声道音量加
   - c 右声道音量减


alsa控制[alsactl]
-----------------

配置好声卡后，直接关机后配置会消失, 因此通过此工具保存

- 保存配置

.. code-block:: bash

   alsactl -f /var/lib/alsa/asound.state store

- 加载配置

.. code-block:: shell

   alsactl -f /var/lib/alsa/asound.state restore


录音工具[arecord]
-----------------

- 录制一段音频

::

   arecord -f cd -d 10 test.wav

.. note::

   - -f 指定音频格式, cd表示cd级别音频
   - -d 指定录制时长



.. function:: foo(x)
              foo(y, z)
   :module: some.module.name

   返回用户输入的一行文本.a



``hello world``

Lorem ipsum [Ref]_ dolor sit amet.

.. [Ref] 参考文献, 书,URL 等.

.. code-block:: bash

  [root@mail ~]# cat /etc/shells
  /bin/sh
  /bin/bash
  /sbin/nologin
  /usr/bin/sh
  /usr/bin/bash
  /usr/sbin/nologin
  /bin/tcsh
  /bin/csh


.. py:function:: enumerate(sequence[, start=0])

   返回一个迭代器，输出包含索引及*sequence*里所有条目的元组.

