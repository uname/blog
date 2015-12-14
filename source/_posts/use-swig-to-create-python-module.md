title: 使用swig创建Python模块
date: 2015-12-14 16:50:40
tags: 代码笔记
---
做Python开发，有时出于性能考虑或需要调用已有C/C++实现的东西时，我们就要使用Python的ctypes模块提供的CDLL方法或者自己编写扩展模块。
ctypes的CDLL方法对不含有C++类的dll或者so的访问还是比较方便靠谱的，但是对C++类方法的访问似乎不太好用。这时我们就需要自己编写扩展模块了。

编写扩展模块可以使用Python提供的API从0开始实现（比较累），也可以借助boost、swig或者sip等工具库来实现。知名度很高的PyQt4（Qt4的Python绑定）就是使用sip来完成的。
这里仅记录使用swig创建Python扩展模块的方法，以便日后翻查。

开始前需要先安装swig和python开发环境，笔者在ubuntu下使用如下命令完成：
```shell
sudo apt-get install swig python-dev

```

