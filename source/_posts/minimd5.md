title: minimd5
date: 2015-11-26 15:28:04
tags: 小工具
---

在kindle电子书的源码中，util-linux_2.12r\lib目录下有md5.c和md5.h两个文件。md5.c实现了计算md5值的算法，且没有外部依赖。md5.h内有一行
```C
#include "../defines.h"
```
实际上并不需要defines.h，注视掉即可。利用这两个文件，我们可以实现一个很小的md5计算器，我把它叫做minimd5。源码也灰常简单。已经放在Github上**[minimd5](https://github.com/uname/minimd5)**

效果如图：

![](/images/minimd5/snapshort.png)
