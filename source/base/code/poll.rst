IO多路复用
==========

1 select
--------

1.1 说明
********

1.2 参考代码
************

.. code:: c

   #include<sys/select.h>

   int main(void)
   {
       int ret;
       fd_set fds;

       while(1)
       {
           FD_ZERO(&fds);
           FD_SET(xxx_fd, &fds);

           ret = select(xxx->fd + 1, &fds, NULL, NULL, -1);
           if(ret == -1)
               continue
           if(ret == 0)
               exit(1)

       }
   }

2 poll
------

2.1 说明
********

2.2 参考代码
************

.. code:: c

   #include <poll.h>
   #include <unistd.h>

   int main(void)
   {
       int fd, poll_num;
       struct pollfd fds[2];
       nfds_t nfds;

       nfds = 2;

       fds[0].fd = STDIN_FILENO;
       fds[0].events = POLLIN;

       fds[1].fd = fd; // 要监视的文件描述符
       fds[1].events = POLLIN;

       while(1)
       {
           poll_num = poll(fds, nfds, -1);

           if(fds[1].events & POLLIN)
           {
               deal_work
           }
       }
   }

3 epoll
-------

3.1 说明
********



3.2 参考代码
************

.. code:: c

   #include <sys/epoll.h>

   int main(void)
   {
      int epfd, devfd[3];
      struct epoll_event event_param;

      /* 创建epoll描述符 */
      epfd = epoll_create(1);

      /* 添加要监视的描述符 */
      event_param.events = EPOLLIN | EPOLLET;

      event_param.data.fd = devfd[0];
      epoll_ctl(fd, EPOLL_CTL_ADD, devfd[0], &event_param);

      event_param.data.fd = devfd[1];
      epoll_ctl(fd, EPOLL_CTL_ADD, devfd[1], &event_param);

      event_param.data.fd = devfd[2];
      epoll_ctl(fd, EPOLL_CTL_ADD, devfd[2], &event_param);

      for(;;)
      {
          epoll_wait(epfd, &event_param, 1, -1);
      }
   }


