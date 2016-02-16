title: NodeMCU上手笔记
date: 2016-01-06 14:53:02
tags:
---
笔者最近想把开发板的高速传感器数据通过无线的方式传输到PC端进行波形分析，需要一个串口到WiFi的透传模块。之前笔者一直使用路由器刷入OpenWRT的方式来做串口和WiFi的透传，这样摆脱了调试线，又有不错的通信速度，效果还是不错的。

之前了解过NodeMCU，看介绍它也有WiFi和串口，应该可以满足需求，于是在淘宝买了一块做了实验。关于NodeMCU，笔者这里简单说下：
NodeMCU是一个基于esp8266的开发板，官方的说法它是一个超简单的物联网开发平台。确实，它有WiFi和串口，上可以连接云主机，下可以通过串口或I2C与其他传感器通信。不过笔者的试用感受，这东西玩玩就行了，很难想象它将来会在物联网上有什么大的成就。
不过笔者比较欣赏的是它提供了一套Lua的API，这对之前没有搞过硬件开发的纯软程序员来说是个比较好的切入点。
它的官网：http://www.nodemcu.com/
它在Github的开源地址：https://github.com/nodemcu/nodemcu-firmware
（笔者之前也一度想给STM32开发一套Lua的API，一直没有时间精力去搞 - - !）


一般从淘宝买回来的时候，卖家已经帮忙刷好了固件。不过自己还是要熟悉下过程，再刷一遍：
######1. 安装串口驱动#######
如果购买的是esp8266模块，则需要一个TTL转串口的模块，按照官方文档接好Rx，Tx，并安装好串口驱动。
如果购买的是已经做好的NodeMCU开发板，那USB连接到电脑，并安装好串口驱动就好了。

######2. 刷固件#######
去NodeMCU的Github项目地址去下载它的固件nodemcu_xxx.bin和烧写工具ESP8266Flasher.exe
https://github.com/nodemcu/nodemcu-firmware/releases
https://github.com/nodemcu/nodemcu-flasher

打开ESP8266Flasher，选择NodeMCU使用的串口（一般ESP8266Flasher会自动选择该串口，如果你只连接了这个一个串口设备）。
在Config页中的第一个选择框处选择已下载的固件bin文件。
![](/images/nodemcu/nodemcu1.png)
然后切换到Operation页中单击Flash按钮开始烧写。大概3分钟烧写完成，如下图所示：
![](/images/nodemcu/nodemcu2.png)

######3. 测试#######
打开串口工具，选择NodeMCU的串口号，波特率默认9600（暂时没有找到修改方法，可能需要重新编译固件）。
打开后，NodeMCU会输出几个乱码（没有的话，可以reset下板子）然后版本号、一行错误提示，和输入提示符。如图：
![](/images/nodemcu/nodemcu3.png)
我们输入一行Lua代码：
print("Hello NodeMCU, This is uname")
可以看到NodeMCU正常执行了该打印语句。

######4. 如何做串口到WiFi的串口#######
至少有两种方式，一是修改固件代码，从esp8266提供的C API层搞起编译一个自己的专有透传固件。前端时间在一个STM32的论坛看到，已经有人这么做了（不过是Socket通信是UDP，不推荐）。
另一种方式就是使用Lua的API写自己的穿透脚本，然后通过串口写入NodeMCU。这样的方式相对简单易行，但有一个问题，如何让自己的脚本在板子启动时自动执行。
我们仔细看下上图中的那行错误提示：
lua: cannot open init.lua
实际上NodeMCU上电后默认会执行init.lua中的代码，因为刷入的固件中并没有init.lua这个文件，所以才会出错。
我们先尝试写一行打印语句到init.lua中看看是不是会在上电后默认执行：
![](/images/nodemcu/nodemcu4.png)
可以看到，我们写入init.lua中的代码在上电时自动执行了！
于是，我们可以参考官方给出的Lua API：https://github.com/nodemcu/nodemcu-firmware/wiki/nodemcu_api_cn
写自己的透传脚本了。

######5. 怎么上传脚本到NodeMCU开发板#######
刚刚没有交代。这里补上，如上图，通过Lua API提供的file模块即可将代码一行一行的写入init.lua。
你需要了解一些Lua的语法。

######6. 透传脚本#######
笔者已经写好了用于透传的init.lua脚本，并写好了用于上传init.lua的flash.py脚本和用于删除已上传init.lua的clean.py脚本（若要使用该上传和删除脚本，需按照Python环境和pyserial模块）。
- init.lua：
![](/images/nodemcu/init.lua.png)

不要按照上图重新敲代码，所有代码和工具我都已上传到我的github：https://github.com/uname/nodemcu-sernet-proxy

使用flash.py上传init.lua：
![](/images/nodemcu/upload.png)

上传完成后重新在串口工具中打开串口：
![](/images/nodemcu/nodemcu6.png)

此时你从串口敲入的Lua语句不再被执行，而是直接转发到通过TCP连接到NodeMCU服务器的客户端！
以后该脚本一上电，init.lua脚本执行，进入透传模式。再也无法像刚开始那样愉快的输入Lua语句执行了，如果要回到之前那样，执行执行clean.py脚本。

记住一下几点：
- 透传模式下NodeMCU的WiFI工作在AP模式，网络SSID一般为ESP_XXXXXX，密码为空
- 串口波特率貌似只能为9600
- 串口发送的数据必须以\r或\n结尾开会被转发（转发出去的数据中会去除\r\n）

玩Arduino的同学可以用这个转发脚本搞一个WiFi控制的小车哦：）

