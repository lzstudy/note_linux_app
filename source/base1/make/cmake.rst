CMAKE笔记
=========


添加编译选项
-------------

- 添加编译选项

::

   set(CMAKE_C_COMPILER "arm-linux-gnueabi-gcc")
   add_compile_options(-std=c11 -Wall)

添加源文件
----------

- 添加源文件夹

::

   aux_source_directory(. srcs)
   aux_source_directory(light_switch srcs)

   # 生成可执行文件/库
   add_executable(main ${srcs})
   target_link_libraries(main ${libs})

添加头文件
----------

- 添加头文件夹

::

   include_directories(${PROJECT_SOURCE_DIR})

添加库文件
----------

::

   link_directories(${PROJECT_SOURCE_DIR}/alsa_lib)
   set(libs pthread asound)

.. note::

   $CC test.c -o main -Ixxx -Lyyy -lzzz

   - xxx头文件路径
   - yyy链接库路径
   - zzz链接库名字
   
