常用规则
========

1 常用变量
----------

1.1 自动变量
------------

- ``$@`` 表示目标 
- ``$<`` 表示第一个依赖文件

1.2 VPATH
---------

默认make只会在当前目录中寻找依赖文件和目标文件, 如果使用 ``VPATH`` , 那么就会到指定的目录中寻找

.. code:: shell

   VPATH = src:../headers

上面定义两个目录, "src"和"../headers", 目录由 ``:`` 分隔

1.3 vpath
---------

更为灵活的一种使用

.. code:: shell

   vpath %.h ../headers
   vpath %.c foo:bar

1.4 MAKELEVEL
-------------

表示make有嵌套动作, 表示当前的层级

1.5 CFLAGS
----------

编译选项


2 常用函数
----------

2.1 wildcard
************

wildcard用于明确表示通配符, 在makefile里, 变量的本质是宏, 如果objs = *.o, 那么objs的值就是*.o
而不是所有的.o文件, 如果想表示, 那么就需要wildcard功能

.. code::

   src = $(wildcard *.c /usr/src/*.c)

   all:
      @echo $(src)

   会输出hello.c world.c /usr/src/aa.c /usr/src/bb.c

2.2 patsubst
************

patsubst用于将文件模式进行替换

``$(patsubst 原模式, 目标模式, 文件列表)``

.. code::

   # src中的.c替换为.o

   all:
       @echo $(patsubst %.c, %.o, $(src))


2.3 subst
*********

subst用于字符串处理, 用法 ``$(subst FROM, TO, TEXT)``, 将TEXT中的内容从FROM变为TO

.. code::

   test1 = $(subst a, b, this is a end)
   test2 = $(subst a, , this is end)

   all:
       @echo $(test1)
       @echo $(test2)

   # test1 - test is b end, 把a替换为b
   # test2 - test is end, 把a删除










