backtrace
=========

1 简介
------

使用backtrace打印出栈帧所在的地址, 然后利用addr2line获取详细的信息

2 编译部分
----------

2.1 编译简单程序
****************

.. code:: c

   arm-linux-gnueabi-gcc -funwind-tables ***.c -o main

2.2 编译带有动态库调用的程序(想要调试动态库)
********************************************

.. code:: c

  arm-linux-gnueabi-gcc -funwind-tables -rdynamic ***.c -o main


.. note::

   * -funwind-tables 会生成栈回溯相关的信息
   * -rdynamic 会把所有的符号编程动态符号, 此时就可以调试动态库了

.. tip::

   编译后的程序可以使用strip来减小程序, 不过此时只能打印寄存器地址, 无法打印调用符号

3 使用方法
----------

3.1 addr2line定位应用程序
*************************

.. code:: c

   arm-linux-gnueabi-addr2line -C -f -e main <reg1> <reg2> <reg3> ....

* -C 显示全路径
* -f 显示行号
* -e 指定应用程序名称, 否则使用默认a.out

3.2 addr2line定位动态库
***********************

由于动态库的地址是随机的, 因此需要先计算动态库所在基地址, 程序中可通过如下方法获得

.. code:: c

   # 打印当前程序使用的所有动态库
   char buff[64] = {0x00};
   sprintf(buff,"cat /proc/%d/maps", getpid());
   system((const char*) buff);

   # 找到出问题动态库的基地址

   # 用栈回溯的问题地址 - 基地址获取到偏移

   # 用3.1的方法计算动态库代码位置

.. note::

   貌似程序崩溃后, 手动 cat /proc/xxx/maps 来查看库的地址

4 参考代码
----------

4.1 空指针代码
**************

.. code:: c

   int *p = 0;
   *p = 1;

4.2 backtrace代码
*****************

.. code:: c

   #include <stdio.h>
   #include <stdint.h>
   #include <unistd.h>
   #include <signal.h>
   #include <execinfo.h>

   #define BACKTRACE_SIZE   16

   static void exception_handler(int signo)
   {
       int layer;
       void *stack_frame[BACKTRACE_SIZE] = {0};

       /* 获取栈帧 */
       layer = backtrace(stack_frame, BACKTRACE_SIZE);

       /* 打印栈回溯 */
       printf("\n-------------------------------------------- cut here --------------------------------------------\n");
       printf("backtrace %d layer\n", layer);
       printf("get more info:\n");
       printf("\n     arm-linux-gnueabi-addr2line -C -f -e airdocApp ");
       for(uint8_t i = 1 ; i < layer ; i++)
           printf("%p ", stack_frame[i]);
       printf("\n\n--------------------------------------------------------------------------------------------------\n");
 
       /* 恢复信号处理 */
       signal(signo, SIG_DFL);
       raise(signo);
   }

   int main(void)
   {
       /* 注册异常信号 */
       signal(SIGSEGV, exception_handler);
       signal(SIGABRT, exception_handler);
       signal(SIGFPE, exception_handler);
       signal(SIGSEGV, exception_handler);

       while(1)
       {
           sleep(1);
       }
   }


