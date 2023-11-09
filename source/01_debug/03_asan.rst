ASan
=================

1 组件介绍
-------------

   结合编译器插桩和运行时快速内存检测工具, 主要用于检测代码中内存安全的问题, 可解缓冲区溢出、
   空指针引用、悬垂指针、非法内存释放、栈变量在作用域失效后使用、栈变量在函数体返回后使用、
   全局变量初始化顺序检测(global-init-order). 

2 工作原理
-------------

   - Instrument静态插桩模块对栈上对象、全局对象、动态分配的对象分配redzone, 
   - runtime运行时库替换malloc / free /memcpy /memset灯实现

3 使用方法
-------------

.. code:: c

   # 只需要在编译时加入下面的编译选项即可
   -fsanitize=address,pointer-compare,pointer-subtract -fno-omit-frame-pointer

.. tip:: 
   
   可能需要移植的动态库

   - libalibasan.so.2 => libasan.so.2.0.0
   - libasan.so.2.0.0

