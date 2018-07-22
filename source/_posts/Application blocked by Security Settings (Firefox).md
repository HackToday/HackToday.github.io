title: Application blocked by Security Settings (Firefox)
date: 2015-02-09 08:08:06
tags: Java
---


						最近访问IMM，发现浏览器中启动remote control，老是退出，提示
Your security settings have blocked an untrusted application from running

这个问题实际上不是firefox中的security设置问题，而是java的security设置导致。

参见这个帖子
https://support.mozilla.org/zh-CN/questions/983382

使用Configure Java，security tab下，将对应的URL加入exception site list中即可。                                   