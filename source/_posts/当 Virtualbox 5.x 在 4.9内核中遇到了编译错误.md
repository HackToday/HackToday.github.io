title: '当 Virtualbox 5.x 在 4.9内核中遇到了编译错误'
date: 2017-02-16 08:36:05
tags: 虚拟化
---

在使用 VBox 安装的 Fedora 25 虚拟机，安装Guest Addition 过程中，发现无法安装，报编译错误

查看Vbox 官方问题列表发现 https://www.virtualbox.org/ticket/16286
这个在 5.1.12 修复了，具体changelog 参考列表
https://www.virtualbox.org/wiki/Changelog

解决方法，升级5.1.12 或者最新的 5.1.14 版本

另外附上：具体常规的安装步骤如下：

```
dnf update kernel*

reboot
```

从 GUI 上选择安装增强功能

```
mkdir /media/VirtualBoxGuestAdditions
mount -r /dev/cdrom /media/VirtualBoxGuestAdditions

dnf install gcc kernel-devel kernel-headers dkms make bzip2 perl

cd /media/VirtualBoxGuestAdditions

./VBoxLinuxAdditions.run
```

参考：

https://www.if-not-true-then-false.com/2010/install-virtualbox-guest-additions-on-fedora-centos-red-hat-rhel/
