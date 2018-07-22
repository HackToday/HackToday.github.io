title: ubuntu默认的dash问题
date: 2012-01-06 00:28:50
tags: LINUX
---


						有时别人写好的shell脚本，拿到ubuntu下执行会出现莫名其妙的错误，Syntax error: "之类的，源自于， ubuntu采用了 效率高的dash，而不是传统的bash，功能相比bash要少很多，语法严格遵守POSIX标准。所以别人机器调试好的shell脚本跑到dash下就有问题，1. 查看：ls -l /bin/sh 发现：/bin/sh -> dash2. 更改：运行 sudo dpkg-reconfigure dash之后选择“No”，参考：1. http://blog.chinaunix.net/space.php?uid=14753126&do=blog&id=4293232. http://www.igigo.net/archives/169                                   