APP框架
==============

OVERVIEW
-------------

    针对顶层应用制作的框架, 以便让文件夹组织更加有序和程序逻辑清晰。
    将文件目录分为3类 : ``main文件``, ``subx(子模块)``, ``common(通用文件)``, 
    main文件是程序的基本运转逻辑业务, 其余皆以模块的形式分布在
    subx的文件夹中。
    
    模块初始化采用自动初始化功能, 每个subx子模块编译为静态库, 最终和main文件链接到一起
    生成可执行文件, 为了框架性, 需要subx文件夹名称和编译出的静态库名称相同。


1 组件初始化链接脚本生成
--------------------------

    生成链接脚本命令为 ``ld -verbose``, 生成后删除最上面和最下面的注释即可使用.

.. code-block:: c

    # arm平台
    arm-poky-linux-gnueabi-ld -verbose

    # 应用平台
    ld -verbose

2 在CMake中引用自定义脚本
---------------------------

.. code-block:: c

    # 设置一个变量 LINK_SCRIPTS来保存链接脚本全路径(一定要全路径)
    set(LINK_SCRIPTS "-T/home/zw/zwapp/app_framework/common/link.lds")

    # 链接库时引用此变量
    target_link_libraries(${target} ${libs} ${LINK_SCRIPTS})

3 链接脚本自动初始化代码段
------------------------------

    在text段的最后加入自定义段, 可以使用 ``nm -n a.out`` 来查看函数是否安装预期排序

.. code-block:: 
    
    /* 自定义 */
    __init_fn_start = .;
    KEEP(*(SORT(.init_fn*)))
    __init_fn_end = .;

4 应用顶层目录参考Cmake
-----------------------------

.. code-block:: c

    #指定最低版本
    cmake_minimum_required(VERSION 3.10)

    # 设置子目录和依赖库 - [根据情况修改]
    set(libs pthread)
    set(subdir common sub1 sub2)

    # 设置目标名称
    set(target main)
    project(${target})

    # 设置编译变量
    #set(CMAKE_C_COMPILER "arm-linux-gnueabi-gcc")
    set(CMAKE_C_COMPILER ${CC})
    set(CMAKE_AR ${AR})
    set(LINK_SCRIPTS "-T/home/zw/zwapp/app_framework/common/link.lds") 

    # 设置编译选项
    add_compile_options(-std=gnu11 -Wall -g -O1)
    # add_compile_options(-fsanitize=address -fno-omit-frame-pointer)
    # add_compile_options(-Werror)

    # 包含目录和添加子文件夹(条件:静态库与文件夹同名)
    foreach(dir ${subdir})
        include_directories(${dir})
        add_subdirectory(${dir})
        set(libs ${libs} ${dir})
    endforeach()

    # 添加main文件
    aux_source_directory(${PROJECT_SOURCE_DIR} srcs)

    # 添加库路径
    # link_directories(${PROJECT_SOURCE_DIR})

    # 生成可执行文件
    add_executable(${target} ${srcs})
    target_link_libraries(${target} ${libs} ${LINK_SCRIPTS})


5 应用子目录参考Cmake
-----------------------------

.. code-block:: c

    # 设置目标名字和依赖库 - [根据需要修改]
    set(target common)
    set(libs pthread)

    # 编译本目录下所有.c文件
    aux_source_directory(. srcs)

    # 生成静态库
    add_library(${target} ${srcs})
    target_link_libraries(${target} ${libs})

6 下载框架
-------------

    通过zwsample下载
