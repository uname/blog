title: 树莓派2通过I2C访问MPU6050
date: 2015-12-20 14:16:47
tags: 树莓派
---
笔者最近做了一个平衡车，主要用到了STM32F10x + MPU9250 + FreeRTOS。
手边正好还有一块MPU6050的模块和树莓派2，就趁热打铁先把树莓派访问MPU6050的路走通，以便后面需要时查阅。

###### 1. 首先要启动树莓派的I2C功能 ######

sudo mount /dev/mmcblk0p1 /mnt/
cd /mnt
sudo vim config.txt

找到下面两行
![](/images/pi2-I2C-MPU6050/1.png)
将注释符号#删掉，如果没有上面两行则自己编辑保存。然后reboot系统。

###### 2. 安装i2c-tools ######
sudo apt-get install i2c-tools

顺便推荐个国内的树莓派软件源: 
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/   wheezy main contrib non-free rpi
编辑/etc/apt/sources.list，注释掉官方源，添加上面那个。重新sudo apt-get update下。

###### 3. 连接MPU6050设备，并测试I2C驱动是否已经OK ######
接线只需要四根：电源正3.3V，Ground，SCL，SDA。
MPU6050模块上会有标志，与树莓派对齐用杜邦线接好即可。
树莓派2的IO定义如图：
![](/images/pi2-I2C-MPU6050/2.png)

连接好设备后，通过命令：
i2cdetect -l
查看设备是否已正常工作。如果你的树莓派只接了MPU6050一个I2C设备，对应设备文件为：
/dev/i2c-1
![](/images/pi2-I2C-MPU6050/3.png)

至此I2C驱动已开启，设备工作正常，可以继续写代码访问数据了。

------------

###### 4. 代码 ######
Github上有个[MotionSensorExample](https://github.com/rpicopter/MotionSensorExample)用于树莓派对MPU6050/MPU6500/MPU9150/MPU9250的I2C访问。
不过笔者clone了代码，在树莓派2上编译失败了。
有人也遇到了相同的问题并发了issue#
[inv_mpu_lib/inv_mpu.c:392:2: error: expected primary-expression before ‘.’ token](https://github.com/rpicopter/MotionSensorExample/issues/2)
截至到笔者发此文章时，作者似乎也并未修改该问题（其实就是树莓派2上安装版本的g++编译器不支持C99标准的C struct复合字面量初始化问题）。
于是笔者fock了源码，并修复了这个错误。所以这里就用我修改后的[MotionSensorExample](https://github.com/uname/MotionSensorExample)来测试吧。

###### 5. 编译&&测试 ######
ssh或者serial登陆到树莓派2，clone笔者修改后的MotionSensorExample：

git clone https://github.com/uname/MotionSensorExample
cd MotionSensorExample
make

如果一切OK的，会在MotionSensorExample生成一个测试程序mstest，执行mstest观察数据读数。
![](/images/pi2-I2C-MPU6050/4.png)

###### 6. 其他 ######
MotionSensorExample通过条件编译的方式来支持不同的MPU（6050/6500/9150/9250），只要修改位于MotionSensorExample/MotionSenso下的Makefile即可：
CXX_OPTS=-c -DMPU6050 -DMPU_DEBUGOFF -I../libs/

实际上笔者这次测试用的是MPU9250，但是如果编译时使用-DMPU9250，则编译出的程序并不能正确工作，
不过既然-DMPU6050可以拿到yaw, pitch, roll数据，暂时也就够用了。错误定位等到有时间再慢慢看吧。

