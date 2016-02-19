title: 一种嵌入式设备数据的实时无线观察方法
date: 2016-02-19 16:15:05
tags:
---

&emsp;&emsp;笔者有一块STM32的开发板，需要读取MPU6050的姿态数据。这些数据在经过均值和卡尔曼滤波等处理后，得到一些角度数据。这些角度数据对笔者进行的开发调试非常重要，因此需要一种可以实时观察这些数据的手段。
最简单的方式是通过一根串口线连接到PC，打开PC端的串口数据处理软件即可。但这样会有一根数据线，数据线的存在造成的物理抖动会干扰到这些数据，并且会阻碍开发板在更大的空间内自由移动。因此需要寻找无线的解决方案。

&emsp;&emsp;笔者使用如下方式，通过WiFi获取开发板数据并在PC端的RealtimePlotter软件上显示数据波形。
![](/images/com2tcp-debug/solution.png)

1. Serial<->Tcp Bridge用来将开发板的串口数据通过桥接的方式提供给通过TCP连接的COM2TCP。这里的Serial<->Tcp Bridge笔者使用一个运行在安装了OpenWRT系统开发板上的Linux进程（sock2serial）。
2. COM2TCP是一个可以Windows系统上桥接网络和串口通信的工具，[下载地址](http://astrogeeks.com/AstroGeeks/COM2TCP/download.html)。下载完成后打开，可看到如下的设置界面：
![](/images/com2tcp-debug/com2tcp.png)。
将IP和TCP Port配置为Serial<->Tcp Bridge监听的地址和端口。COM Port选择一个未使用的COM口，单击连接即可。
3. 笔者用来观察波形的软件[RealtimePlotter](https://github.com/uname/RealtimePlotter)是github上的一个开源工具，使用Processing语言编写。该软件从串口读取数据并绘制波形，支持最大六个通过的数据绘制。
数据格式为: "value1 value2 value3 value4 value5 value6\r"。有时间可以把这个工具修改为从TCP服务器读取数据，这样就可以省去COM2TCP了。
RealtimePlotter有三个目录，分别为BasicRealtimePlotter、RealtimePlotterArduinoCode和RealtimePlotterWithControlPanel。笔者使用BasicRealtimePlotter，这里需要编辑BasicRealtimePlotter.pde代码，修改在COM2TCP中使用的串口号，如图：
![](/images/com2tcp-debug/comport.png)。

一切准备就绪后，运行BasicRealtimePlotter.pde就可以看到数据了。
有几点需要注意：
1. 原始数据仍通过串口输出，波特率设置高点可以更快传输数据。
2. BasicRealtimePlotter软件的数据绘制能力有限，数据太快太多有可能绘制不过来。

