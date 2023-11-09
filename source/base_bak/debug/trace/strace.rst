strace
=======

1 基础说明
------------

strace可以跟踪pid(每个线程都有pid)

.. note:: 
    
    - 获取进程中所有线程pid方法, ``top -H -p pid``, 列出的即所有线程
    - 程序打印线程pid ``gettid()`` 和 ``getpid()``



2 查看程序状态, 或者查看程序卡死在哪里
---------------------------------------

.. code-block:: shell

    strace -p pid
