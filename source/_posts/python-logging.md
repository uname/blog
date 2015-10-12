title: python的logging模块
date: 2015-09-04 22:41:08
tags:
---
这里不打算详细介绍logging模块的使用方法，因为该模块使用简单，而且网上也有比较多的介绍文章。
仅把个人使用Python写小东西时候的logging模块用法记录下来，以便日后copy
######文件 log.py
```python
#-*- coding: utf-8 -*-
import logging
import sys

logger = logging.getLogger("")
__formatter = logging.Formatter("[%(levelname)-7s][%(asctime)s][%(filename)s:%(lineno)d] %(message)s", "%d %b %Y %H:%M:%S")
__streamHandler = logging.StreamHandler(sys.stderr)
__streamHandler.setFormatter(__formatter)
__fileHandler = logging.FileHandler("debug.log")
__fileHandler.setFormatter(__formatter)

logger.setLevel(logging.DEBUG)
logger.addHandler(__streamHandler)
logger.addHandler(__fileHandler)
```
######使用
```python
from log import logger
logger.debug("this is a debug message")
logger.warning("this is a warning message")
logger.error("this is a error message")
```
######效果
![](/images/python-logging/test.png)
