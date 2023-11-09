v4l2-tools
==========

1 windows调试工具
-----------------

============== ======================================
yuvplayer.exe_ yuv调试工具，可以直接查看yuv文件和视频
============== ======================================

.. _yuvplayer.exe: http://120.48.82.24:9100/note_linux_app/yuvplayer.exe

2 4CC转字符串工具
-----------------

.. code:: c

   const char *fmt2str(unsigned fmt)
   {
       static char retstr[5] = {0};
       memset(retstr, 0, 5);

       retstr[0] = fmt & 0xFF;
       retstr[1] = (fmt >> 8) & 0xFF;
       retstr[2] = (fmt >> 16) & 0xFF;
       retstr[3] = (fmt >> 24) & 0xFF;

       return retstr;
   }
