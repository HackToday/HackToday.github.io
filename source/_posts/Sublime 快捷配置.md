title: 'Sublime 快捷配置'
date: 2013-03-04 22:26:42
tags: WINDOWS
---

最近使用了sublime，前段时间编辑encoding中含有GBK，后来打开发现有问题了，到网上搜索发现，sublime有强大的package安装和管理功能，根据官方的说明，非常容易安装配置。

1. 安装

如果防火墙禁止了对应的应用，那么方式1就无法安装

Installation is through the Sublime Text 2 console.


方式2，手动安装

a. Click the Preferences > Browse Packages… menu entry
b. Browse up a folder and then into the Installed Packages folder
c. Download Package Control.sublime-package  from link:
   https://sublime.wbond.net/Package%20Control.sublime-package
   and copy it into the Installed Packages directory
d. Restart Sublime Text


2. 使用

ctrl + shift +p 命令行模式，输入

Install Package

回车，就可以看到列出的所有package


3. 选择packages，安装下面两个就可以解决切换相应编码的问题了

ConvertToUTF8

GBK Encoding Support”


其他的很多插件有待各位自己摸索...........

参考资料：

1. http://www.fuzhaopeng.com/2012/sublime-text-2-with-gb2312-gbk-support/
2. http://wbond.net/sublime_packages/package_control/installation
