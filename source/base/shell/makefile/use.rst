实际应用
========

1 常用规则
----------

1.1 上一条命令应用到下一条, 使用';'
************************************

.. code::

   all:
       cd /home/zw; pwd

1.2 忽略错误, 使用'-'
*********************

.. code::

   all:
       -mkdir test

.. note::

   此时还会再命令行输出错误信息, 只不过被忽略了. 如果想要不打印错误信息可以加入下面指令。
   ``rm test 2>/dev/null || echo > /dev/null``

1.3 打印进入某个目录, 类似编译内核模块
**************************************

.. code::

   make -w

.. note::

   - 使用 ``-C`` 参数, ``-w`` 默认生效
   - 使用 ``-s`` 参数, ``-w`` 总是失效

1.4 变量使用shell命令赋值
*************************

.. code::

   path := $(shell pwd)

2 通配符
--------

2.1 确定某个文件夹所有的.c文件
******************************

.. code:: shell

   src := $(wildcard *.c)

   all:
       @echo $(src)

2.2 把所有的文件替换, 例如.c替换为.o
************************************

.. code:: shell

   dst = $(patsubst %.c,%.o,$(wildcard *.c))
   dst = $(patsubst %.c, %.o, $(src))

   all:
       @echo $(dst)

3 字符串处理
------------

3.1 字符串替换
**************

把 ``this is a end`` 中的 ``a`` 替换为 ``b``

.. code:: shell

   src = $(subst a, b, this is a end)

4 变量处理
----------

4.1 把某个变量的所有.o替换为.c
******************************

.. code:: c

   # 方法1 动态方法
   foo := a.o b.o c.o
   bar := $(foo:.o=.c)

   # 方法2 静态模式
   foo := a.o b.o c.o
   bar := $(foo:%.o=%.c)

N 其他
------

N.1 使用命令包
**************

.. code::

   define run-yacc
   yacc $(firstword $^)
   mv y.tab.c $@
   endef
