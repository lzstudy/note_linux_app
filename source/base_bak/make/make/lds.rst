链接脚本
============


1 链接脚本模板
--------------

sample_lds.tar.gz_

.. _sample_lds.tar.gz: http://120.48.82.24:9100/note_linux_app/sample_lds.tar.gz


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

5 实现段自动初始化
------------------

5.1 添加usr_init段
******************

打开链接脚本, 添加如下内容

.. code:: c

  .usr_init       :
  {
      __usr_init_start = .;
      __usr_init0_start = .;
      *(.usr_init_0)
      __usr_init1_start = .;
      *(.usr_init_1)
      *(.usr_init_1s)
      __usr_init2_start = .;
      *(.usr_init_2)
      *(.usr_init_2s)
      __usr_init3_start = .;
      *(.usr_init_3)
      *(.usr_init_3s)
      __usr_init_end = .;
  }


