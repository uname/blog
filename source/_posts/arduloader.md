title: Arduloader
date: 2015-07-15 15:13:36
tags:
---
Arduloader 用于烧写HEX文件到Arduino开发板。
该工具使用Python语言开发，界面由PyQt4实现，烧写功能由三方工具avrdude完成。
可将Arduloader理解为avrdude命令行工具的UI外壳，以方便日常使用。
因为由Python开发，该工具是跨平台的。笔者已将该工具开源在Github，欢迎感兴趣的Arduino爱好者下载使用。

*[Arduloader的Github开源地址](https://github.com/uname/Arduloader)*
[下载Windows发布版V1.0Beta](https://github.com/uname/Arduloader/releases/download/V1.0Beta/WinRelease_V1.0Beta.zip)

- Windows平台上如何生成Arduloader.exe

1. 确认你已经安装了Python和对应版本的PyQt4、py2exe。
在Python交互模式下执行如下测试，若没有报错，则环境OK。
![](/images/arduloader/deplibs.png)

2. 下载源码，在DOS命令行窗口中进入源码根目录，执行buildexe.py py2exe
![](/images/arduloader/buildexe.png)

3. 执行OK后，会在当前目录生成build和dist目录，其中build目录可删除，dist下的main.exe即为Arduloader的EXE版本。
将源码根目录下的avrtool目录和boards.txt拷贝到dist目录下，即可使用。
![](/images/arduloader/winversion.png)

-----------------------------------------------------------------------------------------------------------------
- 效果截图
![](/images/arduloader/snapshot.png)