title: '关于virtualbox的 4.2.0的一个问题（guest os redhat 6.4)'
date: 2013-07-20 12:21:45
tags: 虚拟化
---

问题：
如果你使用virtualbox 4.2.0会发现一个问题，安装完redhat 6.4操作系统后，安装增强功能时，无法正常的编译，

```
 error: unknown field ‘reclaim_buffers’ specified in initializer
```

这个是virtualbox的一个bug，参看

https://www.virtualbox.org/ticket/11586

解决方法：

link中提到了，修改

```
sudo vi /usr/src/vboxguest-4.2.0/vboxvideo/vboxvideo_drm.c
```

如下

```
109 #if LINUX_VERSION_CODE < KERNEL_VERSION(3, 6, 0)
110     // .reclaim_buffers = drm_core_reclaim_buffers,
111 #endif
```

然后，运行

```
/etc/init.d/vboxadd setup
```

这样就可以正常的编译了。你不可以运行那个install脚本，因为那样会把修改的再次用原始的代码覆盖，编译还是会失败。

其他问题：

好像这个版本的增强功能，还有其他问题，编译成功也无法使用无缝模式的。
估计这个版本没有对redhat 6.4测试过或者不正式支持，无从得知。
