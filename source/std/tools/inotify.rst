通知机制
========

1 说明
------

inotify 可以实现对文件和文件夹的监听


2 参考代码
----------

.. code:: c

   #include <poll.h>
   #include <sys/inotify.h>
   #include <unistd.h>

   int main(int argc, char const *argv[])
   {
       int fd, wd, poll_num;
       nfds_t nfds;
       struct pollfd fds[2];
       char buf[512];

       # 初始化
       fd = inotify_init1(IN_NONBLOCK);

       # 添加要监控的文件
       wd = inotify_add_watch(fd, "/home/zw/aa", IN_CLOSE_WRITE);

       nfds = 2;
       fds[0].fd = STDIN_FILENO;
       fds[0].events = POLLIN;

       fds[1].fd = fd;
       fds[1].events = POLLIN;

       while(1)
       {
           poll_num = poll(fds, nfds, -1);
           if(fds[1].events & POLLIN) {
                printf("hello world"\n);
                read(fd, buf, sizeof(buf);  // 必须有, 否则会移植触发poll
           }
       }
   }

.. note::

   * 使用vim打开和关闭设备只会触发一次, 原因在于vim是写入缓存然后删除原来文件
   * 可以使用echo 1 > xxx 来进行测试





