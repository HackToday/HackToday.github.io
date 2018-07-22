title: 64位操作系统的java开发环境搭建
date: 2011-12-26 11:17:10
tags: Java
---


						1. 查看CPU支持long mode，# cat /proc/cpuinfo | grep flags | grep ' lm ' 2. cpu支持64位，可以安装32位，64操作系统3. 如果自己的机器安装的是32位操作系统，但是虚拟机上需要安装64位操作系统    注意，需要在BIOS开启CPU的虚拟化支持，否则是无法安装64位操作系统。4. java开发人员一般需要安装jdk，目前的jdk有32位和64位，    对于32位的jdk，如果安装在64位系统上会出现 file not found之类的错误，这是因为你没有安装对应的32位lib，一般需要安装 libc6-i386， ia32-libs，可以参考这个链接 http://ubuntuforums.org/archive/index.php/t-1054621.html     但是必须提醒一点，即使安装java安装成功了，但是后面使用eclipse还是会有点问题。因为  我选择eclipse需要选择64位还是32位，那么   a)   64jdk --》 64位eclipse OK   b)  32jdk --》64位eclipse   failed   c)  32 jdk --》32位eclipse OK， 但是运行会有 failed to load module: /usr/lib/gio/modules/libgvfsdbus.so这是因为eclipse并不是完全用java相关的开发，有一些代码是和操作系统lib相关的，那么64位操作系统默认使用64位lib，所以会出现这种问题。网上搜了很多帖子，还没有找到解决方案。   d)  64jdk --》32位eclipse 没试过，估计failed   最终选择方案a) , ok   3.7的eclipse可以正常工作，那么如果我需要开发32位java程序呢？ eclipse配置会有installed jre这个应该可以设置编译使用的环境。注：以上仅个人的一点实践，如有不对之处，请指出。                                      