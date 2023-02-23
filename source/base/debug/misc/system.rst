系统调试
==========

1 查看CUP数量
---------------

.. code-block:: shell

    # 使用proc
    $ cat /proc/cpuinfo

    # top命令后输入1
    $ top

2 查看内存使用情况
--------------------

.. code-block:: shell

    # 每隔1秒打印一次内存
    $ vmstat 1
