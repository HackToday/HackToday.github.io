title: 'OpenStack Swift Selinux, Permission denied'
date: 2015-06-16 11:04:52
tags: 云计算
---

如果参考社区的文档，在Redhat7.1系统上安装swift，会发现陷入到无穷无尽的

```
 can not access directory or file,  Permission Denied
```

什么原因呢，实际上是selinux context导致的权限问题。

这个是社区的文档
http://docs.openstack.org/kilo/install-guide/install/yum/content/swift-install-storage-node.html

Step6 中，

```
Ensure proper ownership of the mount point directory structure:
# chown -R swift:swift /srv/node
```

实际上这样的权限是

```
#ls -Z /srv/node
drwxr-xr-x. swift swift system_u:object_r:unlabeled_t:s0 sdb1
```

需要执行：

```
#restorecon -R /srv/node
```

然后再查看：

```
#ls -Z /srv/node
drwxr-xr-x. swift swift system_u:object_r:swift_data_t:s0 sdb1
```

这样就不会出现问题了，对于/var/cache/swift 同样适用

下面我做了另外一个实验，不使用本地的实际硬盘，创建一个loop device，使用xfs进行格式化。然后挂载来作为swift的back storage。
关于如何开机自动挂载loop device可以参考【3】

比如，

```
   xfs loop,noatime,nodiratime,nobarrier,logbufs=8 0 0
```

测试发现，如果 的前缀path是/srv/node 开始的，这样是没有问题的，但是如果换做其他的path，比如/etc/swift，
就会出现Permission Denied问题（restorecon 无法解决）

据cschwede说，everything under /srv and /var/run/swift should work
我没有测试/var/run/swift，但是/etc/swift是有问题的，后来ho建议了一个增加mount option的方法，

```
  xfs loop,noatime,nodiratime,nobarrier,logbufs=8,context=system_u:object_r:swift_data_t:s0 0 0
```

这样就解决问题了。



感谢：
swift社区的ho(IRC nickname)的热情帮助解决问题。

参考文献：
1. https://ask.openstack.org/en/question/60539/swift-juno-permission-denied-error/
2. http://wiki.centos.org/HowTos/SELinux
3. http://itekblog.com/mount-iso-using-fstab/
