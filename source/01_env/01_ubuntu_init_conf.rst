ubuntu配置文件调整
===================

1 调整配置文件说明
-------------------

======================= ===============================
source.list             为了快速下载, 调整为国内清华源
vimrc                   更好的vim编辑
bashrc                  支持git显示分支等功能
gitconfig               支持git缩写配置等功能
======================= ===============================

2 配置文件更新
-------------------

可以手动下载配置文件 init_conf.tar.gz_, 也可以通过下面命令直接配置 

.. _init_conf.tar.gz: http://120.48.82.24:21000/class_it/linux/env/init_conf.tar.gz

.. code-block:: c

    # 获取软件
    wget http://120.48.82.24:21000/class_it/linux/env/init_conf.tar.gz

    # 解压安装
    tar -vxf init_conf.tar.gz
    cd init_conf && ./install

    # 更新资源
    sudo apt-get update
    sudo apt-get upgrade
