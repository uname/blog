title: 微代码
date: 2016-03-24 15:39:47
tags: code
---
#####记录一些平时缩写或网上偶遇的代码段，以备不时之需。#####

> Java

1. 单例模式的最佳写法
```java
public class Singleton {

    private volatile static Singleton instance;
    private Singleton () {}
    public static Singleton getInstance() {
        if(instance == null) {
            synchronized(Singleton.class) {
                if(instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

```
