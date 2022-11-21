线程编程
===========

1 线程基础
------------

1.1 常见问题
*************

- pthread_t是一个很大的数字(线程id), 不是top中的pid
- pthread_create的参数必须是全局的, 否则会释放掉
- pthread_exit不用于主线程, 会导致主线程变为僵尸进程

1.2 参考程序
*************

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

1.3 线程接口
**************

==================================================== =======================================================================================================
int pthread_equal(pthread_t tid1, pthread_t tid2);   比较两个线程的ID, 由于PID在不同的架构上可能不同的表示
pthread_t pthread_self(void);                        获取自己的pid
pthread_exit(NULL);                                  单个线程退出, 主线程调用此接口会导致子线程还在, 内存无法回收, 主线程变为僵尸进程. 因此引入pthread_join
pthread_join(pid, NULL);                             阻塞等待某个pid线程退出, 一般用于主线程调用, 等同于进程的waitpid
pthread_cancel(pid);                                 发送取消请求, 但是目标线程不一定会取消, 需要目标线程中有对应的处理事件, 取消后要立刻调用pthread_join
pthread_detach(pid);                                 线程在运行完会进入终止态(直到pthread_join后才会释放资源), 使用此接口后, 不需要再调用pthread_join
==================================================== =======================================================================================================


2 线程同步
------------

举例: 如果银行有100块钱, 同时两个人来取, 就会导致异常. 本质原因是内存数据需要拷贝到处理器, 从而导致不一致

.. tip:: 
    
    - 信号量可以代替互斥锁 + 条件变量



2.1 信号量
**************

.. code-block:: c

    #include <stdbool.h>
    #include <semaphore.h>

    static sem_t sem;

    # 无名信号量初始化
    sem_init(&sem, false, 1);

    # 信号量PV操作 
    sem_post(&sem);
    sem_wait(&sem);

2.2 互斥锁
**************

.. code-block:: c

    #include <pthread.h>

    static pthread_mutex_t lock;

    # 互斥锁初始化
    pthread_mutex_init(&lock, NULL);

    # 保护临界区
    pthread_mutex_lock(&lock);

    pthread_mutex_unlock(&lock);

2.3 读写锁
**************

.. code-block:: c

    #include <pthread.h>

    static pthread_rwlock_t rwlock;

    # 读写锁初始化
    pthread_rwlock_init(&rwlock, NULL);

    # 读锁
    pthread_rwlock_rdlock(&rwlock);
    pthread_rwlock_unlock(&rwlock);

    # 写锁
    pthread_rwlock_wrlock(&rwlock);
    pthread_rwlock_unlock(&rwlock);

    # 注销
    pthread_rwlock_destroy(&rwlock);

.. note:: 
    
    - 所有线程都读取, 那么可以同时进行, 提高效率
    - 只要有一个线程获取写锁, 其他线程无论读写, 都要阻塞


2.4 条件变量
**************

条件变量与信号量的区别在于条件变量一次可以唤醒所有线程, 而信号量一次只能唤醒一个线程

.. code-block:: c

    #include <pthread.h>

    static pthread_mutex_t lock;
    static pthread_cond_t cond;

    # 初始化
    pthread_cond_init(&cond, NULL);

    # 线程1 - 等待条件
    pthread_mutex_lock(&lock);
    while(gval < 2000)
        pthread_cond_wait(&cond, &lock);
    pthread_mutex_unlock(&lock);

    # 线程2 - 唤醒
    pthread_mutex_lock(&lock);
    gval += 100
    /* pthread_cond_broadcast(&cond); */
    pthread_cond_signal(&cond);
    pthread_mutex_unlock(&lock);

    # 销毁
    pthread_cond_destroy(&cond);

2.5 自旋锁
***********

.. code-block:: c

    #include <phtread.h>

    static pthread_spinlock_t spin;

    # 自旋锁初始化
    pthread_spin_init(&spin, PTHREAD_PROCESS_PRIVATE);

    # 保护临界区
    pthread_spin_lock(&spin);
    pthread_spin_unlock(&spin);

    # 销毁
    pthread_spin_destroy(&spin);

2.6 屏障
**************



