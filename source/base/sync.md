# 同步互斥

## 信号量

- 有名信号量(进程间使用)

```c
#include <stdbool.h>
#include <semaphore.h>

# 信号量初始化
sem_t *s = sem_open("mysem", O_CREAT, 0666, 1);   // 第一个进程需要创建
sem_t *v = sem_open("mysem", O_RDWR);             // 第二个进程不需要创建, 只打开

# 信号量PV操作
sem_post(&sem);
sem_wait(&sem);

# 注意有名信号量在编译时需要指定 -lpthread
```

- 无名信号量(线程间使用)

```c
#include <stdbool.h>
#include <semaphore.h>

static sem_t sem;

# 无名信号量初始化
sem_init(&sem, false, 1);

# 信号量PV操作 
sem_post(&sem);
sem_wait(&sem);
```

## 锁

- 互斥锁(阻塞)

```c
#include <pthread.h>

static pthread_mutex_t lock;

# 互斥锁初始化
pthread_mutex_init(&lock, NULL);

# 保护临界区
pthread_mutex_lock(&lock);

pthread_mutex_unlock(&lock);
```

- 读写锁(写少读多)

```c
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
```

- 自旋锁(非阻塞)

```c
#include <phtread.h>

static pthread_spinlock_t spin;

# 自旋锁初始化
pthread_spin_init(&spin, PTHREAD_PROCESS_PRIVATE);

# 保护临界区
pthread_spin_lock(&spin);
pthread_spin_unlock(&spin);
```


## 条件变量

```c
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
```

