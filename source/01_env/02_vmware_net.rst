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


2 tftp配置
---------------

.. code-block:: c

    ############################# 1 安装软件包
    sudo apt-get install tftp-hpa tftpd-hpa
    sudo apt-get install xinetd

    ############################# 2 创建tftp目录
    mkdir /home/zw/tftp_root
    chmod 777 tftp_root

    ############################# 3 修改tftp文件
    sudo vi /etc/xinetd.d/tftp
    -----------------------------

    server tftp
    {
        socket_type = dgram
        protocol = udp
        wait = yes
        user = root
        server = /usr/sbin/in.tftpd
        server_args = -s /home/zw/tftp_root
        disable = no
        per_source = 11
        cps = 100 2
        flags = IPv4
    }

    ############################# 4 启动tftpd
    sudo service tftpd-hpa start

    ############################# 5 修改tftpd-hpa文件
    sudo vi /etc/default/tftpd-hpa
    -------------------------------------

    # /etc/default/tftpd-hpa

    TFTP_USERNAME="tftp"
    TFTP_DIRECTORY="/home/zw/tftp_root"
    TFTP_ADDRESS=":69" 
    TFTP_OPTIONS="-l -c -s"

    ############################# 6 重启tftp服务
    sudo service tftpd-hpa restart
