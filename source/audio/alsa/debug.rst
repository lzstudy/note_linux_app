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


