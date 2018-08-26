title: 'Linux, Windows, Mac之间的文件共享和迁移'
date: 2016-06-20 22:15:20
tags: LINUX
---

最近在打算把原来Mac上面的一些资料迁移到我的Windows机器上，碰到了几个问题纪录一下：

1. exfat， ntfs的问题

最初我打算是用我的移动硬盘作为载体，把Mac上的资料Copy到Windows上，问题来了，原来的移动硬盘格式是NTFS的，Mac下面，
这种格式默认是只支持读，不可写，当然可以借助第三方工具做这个，但是会有相应的格式损坏的风险，所以打算从原生角度开展，
exfat是一种windows visa开始支持的，但是我的Windows机器是XP，exfat的支持微软官方提供了相应的驱动。

2. windows Mac 文件之间传输的网络方式

从windows XP 访问Mac， 根据我的实验，即使在Mac上设置了共享和读写权限，以及保证和XP是同一个workgroup，还是无法访问，
防火墙里也有对应的File sharing 允许规则，参看这个帖子：
https://social.technet.microsoft.com/Forums/en-US/daea5819-f6fa-4273-9203-51051c12f4d5/-winxp-osx-?forum=windowsxpzhchs

网上微软论坛也没给出解决，估计问题比较难定位
类似的参考link，具体没有时间细细研究：

http://forums.macrumors.com/threads/windows-8-1-cant-access-share-on-mac-with-valid-credentials.1807636/
http://forums.macrumors.com/threads/windows-7-can-see-but-not-access-os-x-share.1638263/

从Mac访问windows XP比较简单，按照windows的共享目录的设置，然后打开Finder，Command+K打开，输入smb://,就可以访问了

3. Ubuntu Linux exfat挂载
Ubuntu默认的是没有安装对应的exfat驱动，需要运行下面的命令安装驱动

```
sudo apt-get install exfat-fuse exfat-utils
```

这里为什么提到Ubuntu的exfat，因为最后我采用的方式，是安装了一个Ubuntu 
16.04的虚拟机，然后安装了对应的exfat驱动支持，通过usb挂载方式，这样虚拟机就可以访问了移动硬盘了。
接下来就需要通过虚拟机的共享目录方式(virtualbox 需要安装对应的增强功能，才能挂载对应的vboxsf文件类型)，
实现虚拟机和windows XP主机的文件访问，从而可以将移动硬盘的资料拷贝到windows XP的目录中。

4. 其他的云方案，暂时没有考虑，因为自己的需求比较简单，杀鸡不用牛刀。

参考资料：

1. http://techmeasy.blogspot.com/2012/07/add-support-for-exFAT-in-Windows-XP.html
2. http://www.liqi.name/windows-xp-7-mac-os-x-network-share/
3. http://askubuntu.com/questions/451364/how-to-enable-exfat-in-ubuntu-14-04                                   