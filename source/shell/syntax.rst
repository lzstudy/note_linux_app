基本语法
========

1. 条件判断
-----------

1.1 if判断
**********

.. code-block:: shell

    if [ $# -lt 2 ];then
        echo "usage: zwg <-e/-s/-t> <file>"
        exit 1
    else
        echo "hello"
    fi

.. note:: 
    
    - -lt/-gt/-eq 小于/大于/等于
    - -ge/-le/-ne 大于等于/小于等于/不等于

1.2 case语句
************

.. code-block:: shell

    CMD=$1

    case $CMD in
        -e)
        echo "-e"
        ;;

        rp*)
        echo "rp0, rp1, rp2 ..."
        ;;

        [2-5]
        echo "the num is 2 - 5"
        ;;

        *)
        echo "unknown cmd $ARG"
        ;;
    esac

2 基本运算
-----------

2.1 数学运算
*************

.. code-block:: shell

    INDEX=`expr $i + 3`


3 常用方法
-----------

3.1 不输出错误信息
********************

使用 ``2> /dev/NULL`` 不输出错误信息

.. code-block:: shell

    sed -i "s/xxx/$FULL_LOWER/g" `grep xxx -rl $FILE` 2> /dev/null

3.2 获取脚本所在目录
*********************

.. code-block:: shell

    PATH_BASE=$(cd `dirname $0`; pwd)

3.3 变量嵌套使用
*****************

使用eval来实现变量嵌套使用

.. code-block:: shell

    eval KEY+=$(echo \${${INDEX}})


