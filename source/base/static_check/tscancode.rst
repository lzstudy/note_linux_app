TscanCode
=========

1 下载路径
----------

============= ===============================
TscanCodeOrg_ 官网下载, 代码最新, 功能最全
TscanCode_    本地下载, 下载速度块, V2.15版本
============= ===============================

.. _TscanCodeOrg: https://github.com/Tencent/TscanCode
.. _TscanCode: http://120.48.82.24:9100/note_linux_app/TscanCode.tar.gz

2. 源码介绍
-----------

2.1 顶层目录说明
****************

.. code:: bash
   
   root/TscanCode/$ ls
   CHANGELOG.md  LICENSE  README.md  release  samples  trunk

* release目录存放了不同平台的可执行文件(TscanCode正常不需要编译, 直接使用即可)
* samples目录各种问题代码, 用来测试
* trunk是TscanCode的源码, 如果有平台不支持, 那么进入此目录进行编译(直接make即可)

2.2 平台目录说明
****************

默认的SDK里面包含了全部代码, 实际上根据需要我们只需要自己平台的. 这里以linux为例

* TscLua - 分析 ``Lua`` 语法
* TscSharpV1.14.667.linux - 分析 ``csharp`` 语法
* TscanCodeV2.14.2395.linux - 分析 ``c`` 和 ``c++`` 语法

我们使用的是C, 所以拷贝这部分代码到指定目录

.. code:: bash

   cp TscanCodeV2.14.2395.linux ~/tscancode

2.3 c代码检测目录说明
*********************

c代码目录下包含一个 ``cfg`` 文件夹和 ``tscancode`` 文件. 

.. code:: shell

   tscancde$ ls
   cfg tscancode

* tscancode 加权限后可以直接运行

.. tip::

   cfg/cfg.xml 来选择使用哪些规则, value = 1表示启用, value = 0 表示不使用

3 使用tscancode
---------------

.. code:: c

   # 直接使用
   ./tscancode --xml --enable=all -q -j16 xxx.cpp/xxx_dir > scan_result.xml 2>&1

   # 制作成脚本
   $1 = PATH
   ./tscancode --xml --enable=all -q -j16 $PATH > scan_result.xml 2>&1

   # 生成扫描文件scan_result.xml, 建议使用excel看
 
参数说明:

* --xml 输出格式为xml格式
* --enable=all, 使能所有规则, 也可以--enable=xxx来使能部分规则, 具体看help
* -q 不打印进度处理, 只输出结果

