子午工具
=============

1 说明
-------------
   
.. code-block:: shell

   # 下载地址, 使用前需要执行 zwtool -init 来初始化
   git clone git@github.com:lzstudy/zwtools.git


======== ============= ========= ===========================
类型      指令          简化指令   说明
远程类    zwget         gt        scp 获取
...      zwput         pt        scp 推送
...      zwssh         zsh       ssh 远程登录
模板类    zwsample      sp        各种例程模板
功能类    zwstr         str       字符串处理工具
...      zwtrace       zt        内核ftrace工具
======== ============= ========= ===========================

2 远程类
--------------

   使用此类工具, 第一次需要使用gt/pt/zsh -edit 来修改你的ip地址和share路径

.. code-block:: c

    ## 推送文件到服务器share目录
    pt -a xxx

    ## 获取服务器文件到本地
    gt -a xxx

    ## 远程登录服务器
    zsh -a


======== ======================
-a       我的服务器
-s       公司ip
-e       板子ip
-t       临时ip
-edit    添加远程IP
======== ======================


3 模板类
--------------

4 功能类
--------------
