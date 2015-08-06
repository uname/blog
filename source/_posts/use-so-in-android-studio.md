title: 在Android Studio中使用so
date: 2015-08-06 18:51:17
tags:
---
与Eclipse不同，Android Studio中使用so时需要手动添加依赖关系(至少截至到AS1.3版本时还是这样的)。
比如在使用腾讯地图SDK时，我们需要将libtencentloc.so文件拷贝到app的libs目录下(你也可以拷贝到别处)，然后在app的build.gradle文件中添加代码：
```gradle
    sourceSets {
        main {
            jniLibs.srcDirs = ['libs']
        }
    }
```

项目的结构和app模块的build.gradle文件看起来是这样的：
![](/images/use-so-in-android-studio/1.png)
