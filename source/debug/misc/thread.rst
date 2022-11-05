线程相关
=========

1 查看当前进程有多少个线程
--------------------------

- 每个进程都有一个主线程, 既主线程就是进程
- 修改主线程的名字就会更改进程的名字
- 每个线程都有pid和tid
- strace和gdb跟踪的是pid

1.1 查看有多少个线程 + 名字
****************************

.. code-block:: shell

    $ top -H -p pid

1.2 仅仅查看数量
*********************

.. code-block:: shell

    $ cat /proc/pid/status | grep Threads
    Threads:        2

1.3 查看详细信息
*****************

.. code-block:: c

    # 查看数量(下面表示有两个线程)
    $ ls /proc/pid/task
    1131 1132

    # 查看名字
    $ cat /proc/pid/task/pidx/comm
    zw

1.4 格式化输出信息
******************

.. code-block:: shell

    $ ps H -C a.out -o 'pid tid comm'
    



