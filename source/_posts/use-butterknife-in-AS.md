title: 在Android Studio中使用Butter Knife
date: 2015-08-24 22:20:11
tags:
---
- Butter Knife 是Android View的注解框架，Github地址为https://github.com/JakeWharton/butterknife
类似的注解框架还有
1. [dagger](https://github.com/square/dagger)
2. [androidannotations](https://github.com/excilys/androidannotations)
3. more...

论性能，Butter Knife因为使用了反射，与androidannotations在编译时生成执行代码的方式相比，自然略差。但其使用方式比较简单，对于干掉findViewById来说已经够用了（当然它能做的不止这个）。

#####在Android Studio中，可以按照如下步骤配置Butter Knife
1. 去http://jakewharton.github.io/butterknife/ 下载Butter Knife的JAR包
![](/images/use-butterknife-in-AS/download-jar.png)
2. 拷贝下载的jar文件到AS的lib目录下
![](/images/use-butterknife-in-AS/copy-jar-to-as.png)
3. 编辑app模块的build.gradle文件，添加下图中红色框内的内容
![](/images/use-butterknife-in-AS/write-build-gradle.png)
4.点击AS菜单 Build - Make Project（Ctrl+F9）

若没有出错，就可以使用Butter Knife了。
其官网有详细的使用示例：
http://jakewharton.github.io/butterknife/

小提示：被注解的view对象不可以再使用private修饰了。

<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-66724960-1', 'auto');
  ga('send', 'pageview');

</script>