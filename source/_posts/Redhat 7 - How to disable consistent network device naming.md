title: 'Redhat 7 - How to disable consistent network device naming'
date: 2015-04-07 19:15:29
tags: LINUX
---

Redhat7官方确实给了如何沿用旧的网卡名字的方法，但是如果按照上面的操作，发现有的无法实现，不知道是不是受不同hypervisor的影响，
因为我要修改的是一个Vcenter上的虚拟机？ 不太清楚，可能需要问问redhat那边的人，先记录一下这个link方便日后查阅

https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Networking_Guide/sec-Disabling_Consistent_Network_Device_Naming.html

我只使用了最后一种方式

1. vim /etc/default/grub

2. 添加启动参数

```
net.ifnames=0
```

3. 生成新的grub 配置

```
grub2-mkconfig -o /boot/grub2/grub.cfg
```

注意这一步是否基于BIOS还是UEFI会有所不同，参见
https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/System_Administrators_Guide/ch-Working_with_the_GRUB_2_Boot_Loader.html