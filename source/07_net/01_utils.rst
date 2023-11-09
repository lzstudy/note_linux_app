工具程序
==============

获取本机ip
-------------------

.. code-block:: c

    #include <stdlib.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <net/if.h>
    #include <net/if_arp.h>
    #include <arpa/inet.h>
    #include <sys/ioctl.h>
    #include <errno.h>

    static void get_local_ip(void)
    {
        int inet_sock;
        struct ifreq ifr;  
        char ip[32] = {0};

        inet_sock = socket(AF_INET, SOCK_DGRAM, 0);
        strcpy(ifr.ifr_name, "eth0");

        ioctl(inet_sock, SIOCGIFADDR, &ifr); 
        strcpy(ip, inet_ntoa(((struct sockaddr_in *)&ifr.ifr_addr)->sin_addr));

        printf("ip = %s\n", ip);
    }
