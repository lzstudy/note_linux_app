strace
=======

1 组件说明
------------

    strace可以跟踪pid(每个线程都有pid), 可以通过 ``top -H -p pid`` , 列出的即所有线程。
    同时程序中使用 ``gettid()`` 和 ``getpid()`` 来打印出pid信息

2 使用方法
------------

.. code-block:: c

    # 如果程序卡住, 可以使用如下方法查看卡在哪里
    strace -p pid


    