线程同步
========

.. note::


    举例: 如果银行有100块钱, 同时两个人来取, 就会导致异常. 本质原因是内存数据需要拷贝到处理器, 从而导致不一致
    
    - 信号量可以代替互斥锁 + 条件变量



1 无名信号量
-------------

.. code-block:: c

    #include <stdbool.h>
    #include <semaphore.h>

    static sem_t sem;

    # 无名信号量初始化
    sem_init(&sem, false, 1);

    # 信号量PV操作 
    sem_post(&sem);
    sem_wait(&sem);

2 互斥锁
-----------

.. code-block:: c

    #include <pthread.h>

    static pthread_mutex_t lock;

    # 互斥锁初始化
    pthread_mutex_init(&lock, NULL);

    # 保护临界区
    pthread_mutex_lock(&lock);

    pthread_mutex_unlock(&lock);

3 自旋锁
-----------

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

4 读写锁
-----------

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


5 条件变量
-----------

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

6 屏障
-----------
