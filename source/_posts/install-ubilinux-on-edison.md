title: 在Intel Edison开发板上安装ubilinux系统
date: 2016-02-29 19:02:03
tags:
---

<img src="http://www.intel.com/content/dam/www/public/us/en/images/photography-consumer/16x9/edison-board-tilt-16x9.jpg/_jcr_content/renditions/intel.web.576.324.jpg" width="40%"/>
Intel的Edison开发板使用一颗双核双线程500MHz的Intel凌动CPU和一个32位100MHz的Quark MCU。有板载的WiFi和BLE资源。官方的给出的Linux版本为Yocto Linux。
[官方资源链接](https://software.intel.com/zh-cn/articles/intel-edison-developer-resources)。
个人用惯了Ubuntu，感觉Yocto Linux没有那么给力。于是找到了一个非官方推荐的基于Debian "Wheezy"的[ubilinux的系统](http://www.emutexlabs.com/ubilinux)
以下是在Edison上安装ubilinux的步骤。
1. 下载ubilinux, [http://www.emutexlabs.com/files/ubilinux/ubilinux-edison-150309.tar.gz](http://www.emutexlabs.com/files/ubilinux/ubilinux-edison-150309.tar.gz)
2. 将压缩包中的toFlash文件夹解压到本地磁盘
![](/images/install-ubilinux-on-edison/0.png)
3. 因为ubilinux安装过程需要用到dfu-util工具，因此需要下载[dfu for windows](https://cdn.sparkfun.com/assets/learn_tutorials/3/3/4/dfu-util-0.8-binaries.tar.xz)。
笔者开始没有下载该工具直接执行toFlash目录下的flashall.bat脚本时就遇到了如下错误。
![](/images/install-ubilinux-on-edison/1.png)
4. 下载好dfu for windows后将，下载压缩包中的dfu-util.exe和libusb-1.0.dll拷贝到步骤1得到toFlash目录中。
5. 给Edison上电并使用串口登录Yocto Linux环境。
6. 执行toFlash目录下的flashall.bat脚本，然后已登录Edison的串口终端中执行reboot重启设备。此时安装过程就会开始，如图：
![](/images/install-ubilinux-on-edison/3.png)
安装过程中Edison会重启一次，该过程中不要断电，也不要断开PC与Edison的通信线路。
需要特别注意的是，最后一步的Flashing rootfs会等待比较久，耐心等待就好，不要做任何Ctrl+C的打断操作！
7. 安装完成后，Edison会在重启一次。重启完成后在串口终端中使用账户edison/edison登录即可。

后记：没有找到比较快的软件源，apt-get update会比较慢:(
更多的安装细节可以在这里找到
https://learn.sparkfun.com/tutorials/loading-debian-ubilinux-on-the-edison