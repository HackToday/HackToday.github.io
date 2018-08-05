title: 'ubuntu新版本14.10安装问题，及解决方案汇总'
date: 2014-10-26 09:58:03
tags: LINUX
---

Ubuntu今年的最后一个版本14.10如期的发布了，新的版本汇集了一些新的特性，
Ubuntu Desktop： 除了Unity，Xorg 及一些软件包更新的集成外，最有看点的还是
Ubuntu Developer Tools Center,

Ubuntu官方将其定位： 为开发者提供Ubuntu上的快速开发环境，这里的开发环境并不局限在ubuntu应用，涵盖其他的比如Android，Go，
Web 等。 因为是新引入的特性，所以目前支持Android开发环境的快速搭建，期待用户的反馈再开发后续更多的支持。虽然是Ubuntu14.10
引入的官方支持，但是为了更好的LTS优先原则，可以通过PPA添加的方式来在Ubuntu 14.04中使用这个特性， 具体可以参见这篇blog
http://blog.didrocks.fr/post/Ubuntu-loves-Developers

笔者使用Virtualbox安装Ubuntu 14.10出现了如下几个问题，网上也有一些人遇到了，给出解决方法，汇总如下，

1. 安装界面一片混乱，色彩模糊，

http://askubuntu.com/questions/541006/ubuntu-14-10-does-not-install-in-virtualbox

解决方法：

```
Hit Ctrl+Alt+F1 (you will see the shell) and then Ctrl+Alt+F7. You are good to proceed with the installation.
```

2. Virtualbox Guest Additions 不能正确编译安装

这是因为老的版本的问题，下载最新的dkms，然后重新启动机器即可，

http://www.ubuntugeek.com/fix-for-ubuntu-14-10-screen-resolution-issue-on-virtualbox.html

解决方法： 

```
  sudo apt-get install virtualbox-guest-dkms
```

参考资料：

Ubuntu 14.10 Release Notes
https://wiki.ubuntu.com/UtopicUnicorn/ReleaseNotes#Official_flavours

                                   