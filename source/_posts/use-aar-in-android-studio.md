title: 在Android Studio中使用aar
date: 2015-08-03 22:19:19
tags:
---
###什么是aar？
按照Google官方的说法，aar是以二进制形式发布的Android Library Project
aar文件以.aar作为扩展名，而实际上它就是一个zip压缩包，我们将*.aar文件重名为为*.zip，即可用解压缩软件打开（与apk一样）。
其目录结构如下图所示:
![](/images/use-aar-in-android-studio/aar.png)
可以看到aar包含了项目的布局等资源文件。

-----------

###使用aar
我们以github上一个开源的颜色选择器[colorpicker](https://github.com/QuadFlask/colorpicker)为例。
这里我们也不直接编辑build.gradle文件，而是通过IDE提供的菜单入口进行aar文件的加载。
1. 首先下载aar文件。下载的aar文件不必拷贝到你的工程目录下，在后面的加载过程中Android Studio会自动处理。
2. Android Studio中按F4（或者你知道的其他方式）打开Project Structure面板，单击面板左上角的+添加新的Module，如图：
![](/images/use-aar-in-android-studio/as1.png)
3. 在打开的Create New Module面板中选择Import .JAR/AAR Package，单击Next，如图：
![](/images/use-aar-in-android-studio/as2.png)
4. 选择我们下载到colorpicker的aar文件，修改Subproject name为colorpicker，如图：
![](/images/use-aar-in-android-studio/as3.png)
5. 单击Finish，完成Module加载，完成后你会app的同级目录看到以colorpicker命名的字项目，如图：
![](/images/use-aar-in-android-studio/as4.png)
6. 再次打开Project Structure面板，选择app模块的Dependencies页面，为app模块添加Module Dependence，选择刚才加载的colorpicker模块，单击OK。
![](/images/use-aar-in-android-studio/as5.png)
7.如果我们的aar没有更多依赖的话，至此我们就完成了colorpicker.aar的加载，可以愉快的使用了。
但是我们的colorpicker.aar还依赖了github上的[MaterialEditText](https://github.com/rengwuxian/MaterialEditText)，所以我么继续下载MaterialEditText的aar文件，参考上述步骤完成加载。
![](/images/use-aar-in-android-studio/as6.png)
![](/images/use-aar-in-android-studio/as7.png)
8. OK，colorpicker.aar加载OK了，我们在layout中使用colorpicker.aar提供的ColorPickerView测试下效果：
<img src="/images/use-aar-in-android-studio/colorpicker-effect.jpg" width=30% />
