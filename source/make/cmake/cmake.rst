CMake语法
==========

1 基本语法
------------

1.1 模块引用
**************

.camke文件一般是公用的宏/函数

.. code-block:: c

    include(xxx.cmake)

1.2 自定义target
*****************

默认的target会执行make后自动编译, 而自定义的target需要make xxx来编译

.. code-block:: c

    # 定义一个目标clobber, 生成该目标的命令为COMMAND中的内容, COMMENT会在cmake时打印紫色提示
    # 编译make clobber
    add_custom_target(clobber
                    COMMAND ${CMAKE_COMMAND} -E remove_directory ${LIB_ROOT}/${CMAKE_BUILD_TYPE}
                    COMMENT "Delete ${LIB_ROOT}/${CMAKE_BUILD_TYPE}")

1.3 添加子目录并构建
**********************

.. code-block:: c

    add_subdirectory(ebase ${PROJECT_SUB_DIR}/ebase)

1.4 添加源文件
****************

.. code-block:: c

    # 方法1 添加目录

    # 方法2 正则匹配到变量
    file(GLOB src source/*.c)
    file(GLOB inc include/*.h)

1.5 编译目标
***************

.. code-block:: c

    # 1 编译动态库, 会生成libzw.so
    add_library(zw SHARED ${src})

.. code-block:: c

    # 设置目标属性
    SET_TARGET_PROPERTIES(zw PROPERTIES OUTPUT_NAME     ${module})
    SET_TARGET_PROPERTIES(zw PROPERTIES LINK_FLAGS      "-shared -fPIC")


2 if条件判断
--------------

2.1 define判断
****************

用于判断某个变量之前是否定义, set和add_definitions都可以DEFINED
只不过add_definitions更标准

.. code-block:: c

    set(zwvar 0)
    add_definitions(zwvar)

    # 如果定义了zwvar, 则设置为0, 否则设置为1
    # 这里前面的set(zwvar 0)就代表定义了
    if(DEFINED zwvar)
        set(zwvar 0)
    else(DEFINED zwvar)
        set(zwvar 1)
    endif(DEFINED zwvar)

    message("zwvar = ${zwvar}")

2.2 条件判断
****************

- STREQUAL 等于

.. code-block:: c

    if(${zwvar} STREQUAL 1)
        message("aaa")
    elseif(${zwvar} STREQUAL 0)
        message("bbb")
    endif(${zwvar} STREQUAL 1)

.. code-block:: c

    if (SUBDEV_CHAR)
        add_definitions(-DSUBDEV_CHAR)
    endif()

