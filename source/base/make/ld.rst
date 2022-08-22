链接脚本笔记
============


1 链接脚本模板
--------------



2 生成链接脚本
--------------

.. code:: c

   # 1 导出交叉编译工具链, 每个工具链使用的链接脚本不相同
   sourt /opt/fsxxx/envsetup.sh

   # 2 导出默认的链接脚本到当前路径, 因为我们都是基于默认链接脚本修改
   arm-linux-gnueabi-ld --verbose > my.ld

   # 3 默认的链接脚本有一些额外信息, 会导致编译报错
   #   删除开头等号线以前的文件(包含等号线), 删除结尾的等号线

3 使用链接脚本
--------------

.. code:: c

   # 编译
   gcc -c aaa.c -o aaa.o
   gcc -c bbb.c -o bbb.o
   gcc -c ccc.c -o ccc.o
   
   # 链接
   gcc aaa.o bbb.o ccc.o -T my.ld -o out

.. note::

   链接结尾使用gcc aaa.o而没有使用ld命令, 原因在于ld需要自己手动寻找所有文件, 过程非常繁琐

4 在CMake中使用链接脚本
-----------------------

.. code:: c

   set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -T ${CMAKE_SOURCE_DIR}/my.lds")
