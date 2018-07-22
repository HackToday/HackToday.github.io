title: 'hadoop的配置问题（windows下eclipse）'
date: 2011-07-31 12:45:54
tags: WINDOWS
---

配置eclipse和hadoop关联有点费工夫，主要是eclipse for hadoop的插件和eclipse版本有些兼容的关系，比较乱，基本参考：
（1）http://ebiquity.umbc.edu/Tutorials/Hadoop/23%20-%20create%20the%20project.html

里面针对的是hadoop的老版本，如果是0.20.2或最新的0.20.203，那么插件会出现不工作的情况，我用eclipse 3.6，hadoop 0.20.203，没有配成功，主要是在最后的run on  hadoop的问题，具体参考：

（2）http://hi.baidu.com/laxinicer/blog/item/fbaddaf58bdae63fbc3109a0.html

你需要将下载的插件，更名为对应你hadoop版本的文件名，比如你的下载的插件是：
hadoop-eclipse-plugin-0.20.3-SNAPSHOT.jar，
而你的hadoop是0.20.2
那么需要更名为：
hadoop-0.20.2-eclipse-plugin.jar

然后按照（1）操作就行了，那么run on  hadoop的问题就基本解决了。
还有一篇参考，可以看看。
http://hi.baidu.com/nowhere/blog/item/10a18ab1ce580641092302ba.html

这个不兼容真是一个头疼的事情。
