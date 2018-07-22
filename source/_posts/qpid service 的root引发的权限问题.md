title: qpid service 的root引发的权限问题
date: 2013-08-29 22:17:16
tags: LINUX
---


	
使用qpid-tools查看queues的时候，发现老是出现认证无法通过，但是直接qpidd运行没有问题，
查看了一下log发现，/var/log/messages

unable to open Berkeley db /var/lib/qpidd/qpidd.sasldb: Permission denied

检查了一下/var/lib/qpidd/qpidd.sasldb的权限，发现用户为root（rw)，难怪如此，
因为service 启动是以qpidd用户来运行的，没有w的权限。

解决方法：
设置 /var/lib/qpidd 用户和用户组为qpidd:qpidd

其实原来自己写过一篇qpid的setup的文章，都记录要配置这个，只是实际用起来偶犯健忘了。

	
	


	