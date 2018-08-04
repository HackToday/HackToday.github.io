title: 'Registered UUID problem in Virtual Box'
date: 2012-02-18 13:24:15
tags: 虚拟化
---

前段时间,  将虚拟盘从一个分区拷到另外一个分区上，打开虚拟机挂载这个虚拟盘老是报错，

```
VBoxManage: error: Cannot register the hard disk '/media/New Volume/ubuntu-dev/Ubuntu-dev.vdi' {fa106a76-0866-4ab4-8b61-e8a054373555} 
because a hard disk '/media/4E5780F3589D6099/ubuntu-dev/Ubuntu-dev.vdi' with UUID {fa106a76-0866-4ab4-8b61-e8a054373555} already exists
```

搜索发现，原来注册的UUID记录已经存在，UUID嵌入到了VM的这个虚拟盘中，
所以挂载这个转移的分区时候，会检测到UUID 和原来注册的一样，就冲突了。UUID 是用来唯一标志的。

所以解决办法需要重新生成新的UUID，
virtualbox有这个命令，
vboxmanage internalcommands sethduuid  Ubuntu-dev.vdi  
这样vdi虚拟盘救生成了新的UUID，可以挂载了。

很有用的一篇文章，
http://michail.flouris.net/2011/11/virtualbox-vm-disk-clone-uuid-problem/