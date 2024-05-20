文件操作
===================

1 文件计算大小
*********************

.. code-block:: c

    static int store_open(void *store, const char *path, const void **image_data)
    {
        long fsize;
        struct img_store *image = store;

        image->file = fopen(path, "r");
        if (!image->file) {
            syslog(LOG_ERR, "Cannot open the file:%s\n", path);
            return -EINVAL;
        }

        /* 定位文件尾, 计算大小, 在回来 */
        fseek(image->file, 0, SEEK_END);
        fsize = ftell(image->file);
        fseek(image->file, 0, SEEK_SET);

        image->buf = malloc(fsize + 1);
        if (!image->buf) {
            fclose(image->file);
            return -ENOMEM;
        }

        *image_data = image->buf;

        return fread(image->buf, 1, fsize, image->file);
    }