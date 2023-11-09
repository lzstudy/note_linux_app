I2C编程
=========

1 参考模板
-------------

====================== ========================================================================================
i2c_framework.tar.gz_  i2c应用框架模板
====================== ========================================================================================


.. _i2c_framework.tar.gz: http://120.48.82.24:9300/app/hw/i2c/i2c_framework.tar.gz

2 参考代码
-------------

.. code-block:: c

    #include <fcntl.h>
    #include <sys/ioctl.h>
    #include <linux/i2c.h>
    #include <linux/i2c-dev.h>
    

    # 打开设备(i2c总线设备)
    int fd = open("/dev/i2c-3", O_RDWR);

    # 读取寄存器
    # 寄存器有的是双字节, 有的是单字节, 根据情况修改, 这里以单字节为例
    static int i2c_read(uint8_t addr, uint8_t reg, uint8_t *val, uint8_t len)
    {
        struct i2c_msg msg[2];
        struct i2c_rdwr_ioctl_data rid;

        /* 设置要读的寄存器 */
        msg[0].addr  = addr;
        msg[0].flags = 0;
        msg[0].len   = 1;
        msg[0].buf   = &reg;

        /* 设置数据保存信息 */
        msg[1].addr  = addr;
        msg[1].flags = I2C_M_RD;
        msg[1].len   = len;
        msg[1].buf   = val;

        /* 设置消息数量 */
        rid.msgs  = msg;
        rid.nmsgs = 2;

        return ioctl(fd, I2C_RDWR, &rid);
    }
