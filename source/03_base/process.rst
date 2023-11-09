进程编程
====================

1 进程创建
----------------

.. code-block:: c

   #include <unistd.h>

   int main(int argc, char const *argv[])
   {
      int ret;

      /* 创建进程 */
      pid_t pid;
      pid = fork();

      /* 获取进程ID */
      pid_t pid;
      pid = getpid();

      /* 获取父进程ID */
      pid_t ppid;
      ppid = getppid();

      /* 获取实际用户ID */
      uid_t uid;
      uid = getuid();

      /* 获取组ID */
      gri_t gid;
      gid = getgid();

      return 0;
    }



2 进程同步
---------------

.. code-block:: c

   ######################################## 1 发送信号
   # 1.1 进程间发送信号
   #include <sys/types.h>
   #include <signal.h>
   kill(pid, sig);

   # 1.2 给自己发送信号
   #include <signal.h>
   raise(sig);

   # 1.3 alram信号 - 发送SIGALRM
   #include <unistd.h>
   alarm(1);

   # 1.4 pause信号 - 让程序停止运行, 直到捕捉到信号
   #include <unistd.h>
   pause();

   ######################################## 2 信号处理
   struct sigaction sig = {0};
   sig.sa_handler = sig_handler;
   sig.sa_flags = 0;
   sigaction(SIGALRM, &sig, NULL)




3 进程通信
----------------

3.1 有名管道
**************

.. code-block:: c

   #include <stdio.h>
   #include <unistd.h>
   #include <sys/stat.h>

   int main(int argc, char const *argv[])
   {
        int fd_r, fd_w;
        int ret;
        char buf[32];

        /* 创建FIFO */
        ret = mkfifo("./myfifo", 0666);
        CK_RET(ret < 0, ret);

        /* 打开fifo */
        fd_r = open("./myfifo", O_RDONLY);
        CK_RET(fd_r < 0, ret);

        /* 写管道 */
        ret = write(fd_r, "hello", O_WRONLY);
        LOG_I("ret = %d", ret);

        /* 读管道 */
        ret = read(fd_r, buf, sizeof(buf));
        LOG_I("ret = %d", ret);

        return 0;
    }

3.2 消息队列
**************

.. code-block:: c

   #include <stdio.h>
   #include <unistd.h>
   #include <sys/stat.h>
   #include<sys/msg.h>
   #include<sys/ipc.h>

   typedef struct _msgbuf {
        long mtype;
        char mtext[100];
   }msgbuf;

   int main(int argc, char const *argv[])
   {
        int msgid, ret;
        key_t key;
        msg mq;

        /* 生成msgid */
        key = ftok(".",100);
        msgid = msgget(key,IPC_CREAT|0666);

        /* A: 发送消息1 */
        mq.type = 1;
        strcpy(mq.buf, "this is type 1");

        ret = msgsnd(msgid, &mq, sizeof(mq.buf), 0);
        CK_RET(ret < 0, ret);

        /* A: 发送消息2 */
        mq.type = 2;
        strcpy(mq.buf, "this is type 2");

        ret = msgsnd(msgid, &mq, sizeof(mq.buf), 0);
        CK_RET(ret < 0, ret);

        /* A: 发送消息3 */
        mq.type = 3;
        strcpy(mq.buf, "this is type 2");

        ret = msgsnd(msgid, &mq, sizeof(mq.buf), 0);
        CK_RET(ret < 0, ret);

        /* B: 接受任意消息 */
        ret = msgrcv(msgid, &mq, mq.buf, 0, 0);
        CK_RET(ret < 0, ret);

        /* B: 只接受3号类型消息 */
        ret = msgrcv(msgid, &mq, mq.buf, 3, 0);
        CK_RET(ret < 0, ret);
    }

3.3 共享内存
**************

.. code-block:: c

   #include <sys/shm.h>

   int main(int argc, char const *argv[])
   {
        int msgid, ret;
        key_t key;
        char *shmaddr;

        /* 生成shmid */
        key = ftok(".", 100);
        shmid = shmget(key, SHM_SIZE, 0);

        /* 地址映射 */
        shmaddr = (char*)shmat(shmid, NULL, 0);

        /* 去映射 */
        shmdt(shmaddr);
        shmctl(shmid, IPC_RMID, NULL);
        return 0;
    }


3.4 socket
**************


4 守护进程
----------------

::

    - 显示所有的守护进程, -x显示没有终端的进程(既守护进程), -j 显示会话ID、进程组ID, ps -axj
    - []标记的进程表示为内核守护进程
    - 程序运行后加入 &


5 进程数据
----------------

5.1 环境变量
************************

.. code-block:: c

    #include <stdlib.h>

    # 1 使用全部环境变量 - environ
    extern char **environ;
    for (i = 0; NULL != environ[i]; i++)
        puts(environ[i]);

    # 2 获取指定的环境变量 - getenv
    const char *str_val = NULL;
    str_val = getenv("path");
    LOG_I("环境变量的值: %s", str_val);

    # 3 修改环境变量 - putenv
    char *new = "path=hello"
    putenv(new);

    # 4 添加或修改环境变量 - setenv
    setenv("key", "value", 1);

    # 5 删除环境变量 - unsetenv
    unsetenv("name");

    # 6 清空环境变量 - clearenv
    clearenv();


