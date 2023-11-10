网络配置
===================

1 ubuntu网络配置
------------------

1.1 桥接模式与Nat模式区别
******************************

    桥接相当于完全虚拟了一台主机, 有独立的ip地址, 可以与主机通信。而nat模式是主机与虚拟机
    同用一个ip, 主机能上网, 虚拟机就可以上网
    

1.2 配置上网
******************************

1.2.1 修改主机IP
^^^^^^^^^^^^^^^^^^^

    桥接模式想要上网, 必须要保证主机IP地址与虚拟机网段一致, 为此最好将主机IP地址设置为静态的, 
    在win10界面, 配置主机IP ``192.168.31.21``

1.2.2 设置VMvare
^^^^^^^^^^^^^^^^^^^

    通过VMvare软件设置网络模式为桥接, 配置虚拟机IP ``192.168.31.66``

.. code-block:: c

    sudo vi /etc/network/interfaces
    -----------------------------------------

    # auto lo
    # iface lo inet loopback

    auto ens33
    iface ens33 inet static
    address 192.168.31.66
    gateway 192.168.31.1
    netmask 255.255.255.0
    dns-nameserver 8.8.8.8 114.114.114.114


1.2.3 重启网络服务
^^^^^^^^^^^^^^^^^^^

.. code-block:: c

    # 重启网络后即可上网
    sudo service network-manager restart


1.3 配置uboot信息
******************************

    配置uboot的IP ``192.168.31.88``

.. code-block:: shell

    setenv ipaddr 192.168.31.88
    setenv gatewayip 192.168.31.1
    setenv netmask 255.255.255.0
    setenv serverip 192.168.31.66
    setenv ethaddr b8:ae:1d:01:00:00
    saveenv


