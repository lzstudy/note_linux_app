线程基础
===========

1 参考模板
------------

2 参考程序
-------------

.. code-block:: c

    #include <pthread.h>
    #include <sys/prctl.h>
    #include "codestd.h"

    static pthread_t pid;

    /**************************************************************************************************
    * @brief  : 线程入口
    * @param  : None
    * @return : None
    **************************************************************************************************/
    static void *zw_thread(void *arg)
    {
        /* 设置线程名字 */
        prctl(PR_SET_NAME, "zw");
        return 0;
    }

    /**************************************************************************************************
    * @brief  : 线程初始化
    * @param  : None
    * @return : None
    **************************************************************************************************/
    static int thread_init(void)
    {
        int ret;

        /* 创建线程 */
        ret = pthread_create(&pid, NULL, zw_thread, NULL);
        CHECK_RET(ret < 0, ret, "thread create fail err = %d", ret);

        /* 分离线程 */
        pthread_detach(pid);
        return 0;
    }

3 线程接口
-------------

==================================================== =======================================================================================================
int pthread_equal(pthread_t tid1, pthread_t tid2);   比较两个线程的ID, 由于PID在不同的架构上可能不同的表示
pthread_t pthread_self(void);                        获取自己的pid
pthread_exit(NULL);                                  单个线程退出, 主线程调用此接口会导致子线程还在, 内存无法回收, 主线程变为僵尸进程. 因此引入pthread_join
pthread_join(pid, NULL);                             阻塞等待某个pid线程退出, 一般用于主线程调用, 等同于进程的waitpid
pthread_cancel(pid);                                 发送取消请求, 但是目标线程不一定会取消, 需要目标线程中有对应的处理事件, 取消后要立刻调用pthread_join
pthread_detach(pid);                                 线程在运行完会进入终止态(直到pthread_join后才会释放资源), 使用此接口后, 不需要再调用pthread_join
==================================================== =======================================================================================================

4 常见问题
------------

- pthread_t是一个很大的数字(线程id), 不是top中的pid
- pthread_create的参数必须是全局的, 否则会释放掉
- pthread_exit不用于主线程, 会导致主线程变为僵尸进程





