title: 借助方便的autocutsel完成windows7和Linux之间copy-paste
date: 2015-12-01 23:03:14
tags: LINUX
---


						要解决的问题:
有一台windows7 安装了tightvncviewer。有一个远程的Linux xfce4桌面系统。通过vncviewer来远程连接Linux桌面。
因为会交互在两个系统之间操作，所以两个系统之间的copy paste是一个显而易见的需求。

曾经使用过的xcutsel似乎力不从心，发现autocutsel这个工具很好用。
使用:
 sudo apt-get install autocutsel

然后就在Linux终端启动 autocutsel，这样就可以无障碍的copy paste操作了。                                   