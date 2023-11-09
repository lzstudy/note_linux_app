文件编程
====================

1 文件IO
------------

.. code-block:: c

   #include <string.h>
   #include <unistd.h>
   #include <fcntl.h>
   #include "codestd.h"

   int main(int argc, char const *argv[])
   {
      int fd, len;
      char buf[16] = {0};
      const char *str = "zw test fd";

      /* 打开文件 */
      fd = open("../zw.txt", O_RDWR);
      CHECK_RET(fd < 0, fd);

      /* 写入一段内容 */
      len = write(fd, str, strlen(str));
      CHECK_RET_FMT(len < 0, len, "write file fail");

      /* 写完后定位到文件尾, 读取要重新定位到开始*/
      lseek(fd, SEEK_SET, 0);

      /* 读取文件内容 */
      len = read(fd, buf, sizeof(buf));
      CHECK_RET_FMT(len < 0, len, "read file fail");

      LOG_I("len = %d, buf = %s", len, buf);

      /* 关闭文件 */
      close(fd);
      return 0;
   }

2 标准IO
------------

.. code-block:: c

   #include <stdio.h>
   #include <string.h>
   #include <unistd.h>
   #include <fcntl.h>
   #include "codestd.h"

   int main(int argc, char const *argv[])
   {
      int len;
      FILE *fp;
      char buf[16] = {0};
      const char *str = "zw test fp";

      /* 打开文件 */
      fp = fopen("../zw.txt", "r+");
      CHECK_RET(fp == NULL, -1);

      /* 写入一段内容*/
      len = fwrite(str, strlen(str), 1, fp);
      CHECK_RET_FMT(len < 0, len, "write file fail");

      /* 写完后定位到文件尾, 读取要重新定位到开始*/
      fseek(fp, 0, SEEK_SET);

      /* 读取文件内容, fread返回0表示读取到文件结尾 */
      len = fread(buf, sizeof(buf), 1, fp);
      CHECK_RET_FMT(len < 0, len, "read file fail");

      LOG_I("len = %d, buf = %s", len, buf);

      /* 关闭文件 */
      fclose(fp);
      return 0;
   }

3 文件夹操作
------------

.. code-block:: c

   #include <dirent.h>

   int main(int argc, char const *argv[])
   {
      int ret;
      DIR *dp;
      struct dirent *dir; 

      /* 创建文件夹 */
      ret = mkdir("./test_dir", S_IRWXU | S_IRGRP);
      CK_RET(ret < 0, ret);

      /* 打开文件夹 */
      dp = opendir("./test_dir");
      CK_RET(dp == NULL, -1);

      /* 读取文件夹 */
      dir = readdir(dp);
      CK_RET(dir == NULL, -1);

      /* 关闭文件夹 */
      closedir(dir);
      return 0;
   }   


4 权限操作
------------

.. code-block:: c

   #include <unistd.h>
   #include <sys/stat.h>

   int main(int argc, char const *argv[])
   {
      int ret;

      /* 判断某文件是否权限,R_OK, W_OK, X_OK, 返回0表示有 */
      ret = access("./test_file", R_OK);
      CK_RET(ret < 0, ret);

      /* 修改权限 */
      struct stat stat;

      stat("./test_file", &stat);
      ret = chmod("./test_file", stat.st_mode &~S_IXGRP | S_IROTH);

      return 0;
   }

5 所属关系
------------

.. code-block:: c

   #include <unistd.h>

   int main(int argc, char const *argv[])
   {
      int ret;

      ret = chown("./test_file", )

      return 0;
   }

6 文件链接
------------

.. code-block:: c

   #include <unistd.h>

   int main(int argc, char const *argv[])
   {
      int ret;

      /* 创建硬链接 */
      ret = link("./test_file", "./test_new");
      CK_RET(ret < 0, ret);

      /* 创建软链接 */
      ret = symlink("./test_file", "./test_file_link");
      CK_RET(ret < 0, ret);

      /* 删除硬链接/文件 */
      ret = unlink("./test_file");
      CK_RET(ret < 0, ret);

      return 0;
   }

7 文件时间
------------

.. code-block:: c

   #include <sys/stat.h>

   int main(int argc, char const *argv[])
   {
      int ret;
      struct timespec times[2];

      /* 修改时间 */
      ret = utimes("./test_file", times);
      CK_RET(ret < 0, ret);


      return 0;
   }

8 工具接口
------------

.. code-block:: c

   #include <unistd.h>

   int main(int argc, char const *argv[])
   {
      int ret;

      /* 重命名文件 */
      ret = rename("./test_file", "./test_new_name");
      CHECK_RET(ret < 0, ret);

      /* 修改工作目录 */
      chdir("/tmp");

      return 0;
   }



.. tip:: 
   
   文件权限掩码:

   - S_IRWXU = S_IRUSR | S_IWUSR | S_IXUSR
   - S_IRWXG = S_IRGRP | S_IWGRP | S_IXGRP
   - S_IRWXO = S_IROTH | S_IWOTH | S_IXOTH




