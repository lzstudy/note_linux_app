elf分析
==========

1 overview
---------------

:: 

    elf文件组成:
        elf头、程序头、段头、段

    elf文件4种类型:
        目标文件、可执行文件、库文件、核心转储文件

.. code-block:: c

    cat /proc/pid/maps 来查看一个进程的虚拟空间分布


1.1 段类型
*****************

::

    .dynsym [DYNSYM]
        动态符号段, 只在可执行文件中存在, 所有引用动态库的符号都会放在这里
    .dynstr [STRTAB]
        动态字符串符号段, 
    
 
2 常用命令
---------------

2.1 读取elf头
********************



.. code-block:: c

2.2 读取程序头
********************

.. code-block:: c

    readelf -l work.elf


2.3 读取段头
********************

.. code-block:: c

    readelf -S work.elf



2.4 读取所有符号
***********************

.. code-block:: c

    # 方法一(推荐), 读取信息最全
    readelf -s work.elf

    # 方法2
    nm work.elf

    # 方法3
    objdump -t work.elf

.. tip:: 
    
    类型:

    - OBJECT : 表示变量、数据对象
    - FUNC : 函数

    bind:

    - LOCAL 表示局部符号, 外部不可见
    - GLOBAL 表示全局符号, 外部可见
    - WEAK 弱符号


