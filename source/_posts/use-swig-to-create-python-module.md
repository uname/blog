title: 使用swig创建Python模块
date: 2015-12-14 16:50:40
tags: 代码笔记
---
做Python开发，有时出于性能考虑或需要调用已有C/C++实现的东西时，我们就要使用Python的ctypes模块提供的CDLL方法或者自己编写扩展模块。
ctypes的CDLL方法对不含有C++类的dll或者so的访问还是比较方便靠谱的，但是对C++类方法的访问似乎不太好用。这时我们就需要自己编写扩展模块了。

编写扩展模块可以使用Python提供的API从0开始实现（比较累），也可以借助boost、swig或者sip等工具库来实现。知名度很高的PyQt4（Qt4的Python绑定）就是使用sip来完成的。
这里笔者以上一篇文章介绍的[minimd5](http://uname.github.io/2015/11/26/minimd5/)项目为例，记录使用swig创建Python扩展模块的方法，以便日后翻查。
以下步骤中所述源码，可在笔者上传到github上的[minimd5](https://github.com/uname/minimd5)项目中找到

1. 首先安装swig和python开发环境，笔者在ubuntu下使用如下命令完成
```shell
sudo apt-get install swig python-dev

```
2. 编写pyminimd5.c，实现函数: char *md5sum(const char *const file);
![](/images/use-swig-to-create-python-module/pyminimd5.c.png)
3. 编写pyminimd5.i，该文件由swig加载生成pyminimd5_wrap.cxx文件，具体方法看下一步中的Makefile代码片段
```c
%module minimd5
%{
extern char *md5sum(const char *const file);
%}

extern char *md5sum(const char *const file);
```
4. 修改minimd5的Makfile，增加用于编译_minimd5.so的代码
```Makefile
pyminimd5: $(PYMODULE_SOURCES)
    swig -python -c++ pyminimd5.i
    g++ -c pyminimd5.c pyminimd5_wrap.cxx md5.c -I/usr/include/python2.7/ -fPIC
    g++ -shared pyminimd5.o pyminimd5_wrap.o md5.o -o $(PYMODULE_SO)
```

5. make pyminimd5生成_minimd5.so，使用Python加载并测试
![](/images/use-swig-to-create-python-module/test.png)

