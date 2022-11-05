线程与进程
===========

1 进程
---------

2 线程
---------

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
