# 多任务

## 进程

## 线程

```
#include <pthread.h>
#include "codestd.h"

static pthread_t pid;

/**************************************************************************************************
 * @brief  : 线程入口
 * @param  : None
 * @return : None
**************************************************************************************************/
static void *motor_event_monitor_thread(void *arg)
{
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
    ret = pthread_create(&pid, NULL, motor_event_monitor_thread, NULL);
    CHECK_R(ret < 0, ret, "thread create fail err = %d", ret);

    /* 分离线程 */
    pthread_detach(pid);
    return 0;
}
```

