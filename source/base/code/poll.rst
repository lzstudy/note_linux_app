IO多路复用
==========

1 select
--------

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

3.2 参考代码
************

.. code:: c

   #include <sys/epoll.h>

   int main(void)
   {
      int epfd;
      struct epoll_event event_param;

      /* 创建epoll描述符 */
      epfd = epoll_create(1);

      /* 添加要监视的描述符 */
      event_param.events = EPOLLIN | EPOLLET;
      epoll_ctl(fd, EPOLL_CTL_ADD, NULL, &event_param;

      for(;;)
      {
          epoll_wait(epfd, &event_param, 1, -1);
      }
   }


