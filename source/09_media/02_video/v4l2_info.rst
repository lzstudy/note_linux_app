V4L2应用编程介绍
================

1 介绍
------

vl42使用/dev/video与/dev/subdev/与/dev/media设备节点编程, 对于简单的sensor，此时只需要/dev/videox节点即可，对于复杂的sensor需要结合subdev来形成pipeline, /dev/media来控制pipeline. 

=================== =====================
v4l2_mplane.tar.gz_ v4l2 - mplane模板文件
=================== =====================

.. _v4l2_mplane.tar.gz: http://120.48.82.24:9100/note_linux_app/v4l2_mplane.tar.gz


2 参考逻辑
----------

2.2 mplane编程
**************

.. code:: c

   # 1 打开设备
   open(/dev/videvox)

   # 2 查询可用的设备, 用于pipeline存在多摄像头输入的情况
   ioctl(VIDIOC_ENUMINPUT)

   # 3 设置输入构成完整的pipeline
   ioctl(VIDIOC_S_INPUT)

   # 4 查询设备能力
   ioctl(VIDIOC_QUERYCAP)

   # 5 设置帧格式(分辨率 + 数据格式)
   ioctl(VIDIOC_S_FMT)

   # 6 申请帧缓冲
   ioctl(VIDIOC_REQBUFS)

   # 7 查询缓冲区
   ioctl(VIDIOC_QUERYBUF)

   # 8 映射缓冲区
   mmap

   # 9 缓冲区入队
   ioctl(VIDIOC_QBUF)

   # 10 启动数据流
   ioctl(VIDIOC_STREAMON)

   while(1)
   {
      # 11 监听事件
      select/poll

      # 12 取帧
      ioctl(VIDIOC_DQBUF)

      # 13 处理帧
      save_file
    
      # 14 还帧
      ioctl(VIDIOC_QBUF)
   }

   # 15 关闭数据流
   ioctl(VIDIOC_STREAMOFF)










