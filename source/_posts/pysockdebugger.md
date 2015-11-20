title: 开源的TCP/UDP网络通信调试器
date: 2015-11-19 22:32:20
tags: 网络与调试
categories: 我的开源工具
---

笔者在参与的一些项目中，经常需要对使用TCP或UDP通信的协议进行调试。除针对业务协议写一些测试客户端或者脚本外，笔者也会借助网上的现有TCP/UDP调试工具做一些简单的连通和ECHO回包测试。
用过一些工具，总有些不顺手的地方，于是利用琐碎时间，自己用PyQt4做了一个叫**PySockDebugger**的小东西，**已在Github开源**[PySockDebuger](https://github.com/uname/PySockDebuger)（Debugger误写为Debuger，不再修改）。
1.0Beta版本的windows版本已经通过Py2exe打包并上传到Github项目的[release](https://github.com/uname/PySockDebuger/releases)目录下。欢迎下载体验：）
因为使用Python实现，所以在Mac和Linux桌面环境下均可使用（需安装PyQt4，另外有个获取本地IP的小BUG，暂未修改，若使用时遇到，只需按照堆栈错误提示，简单修改下即可）。
主要有以下模式:
1. TCP服务器
2. TCP客户端
3. UDP服务器(主动绑定本地端口)
4. UDP客户端

另外支持在同一个窗口内创建多个实例，不同的实例在Tab页中显示。下过如下图：
![](/images/pysockdebugger/snapshot.png)

除重复发送和UDP客户端模式下的绑定本地端口，这两个功能暂未实现外（代码好写，但暂未有时间），其他功能都测试可用。
后面打算做个插件功能，由用户实现发送和接收数据的pack和unpack过程，这样在调试不同的协议时，只需加载实现了pack和unpack功能的插件即可。而因为PySockDebuger使用Python实现，这样的插件实现起来还是很方便的。
