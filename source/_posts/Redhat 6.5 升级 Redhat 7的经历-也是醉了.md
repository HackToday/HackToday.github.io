title: Redhat 6.5 升级 Redhat 7的经历-也是醉了
date: 2014-12-17 23:06:01
tags: LINUX
---


	1. 

	Redhat 4,5,6之间的upgrade，官方不给与支持，推荐使用fresh install，然后把对应软件的配置和数据迁移到新的server上

	

	Redhat 对一些特定的case给与了支持，但是支持的力度有限

	

	

	

	3. 在Redhat Summit上，

	

	

	

	Redhat官方的升级资料有个可恶的问题就是，给出的升级包没有下载的连接，不知道是不是需要什么subscription number 才能看到？
所以就索性按照Damian Zaremba写的关于 centos 6.5升级到7的步骤执行：
http://damianzaremba.co.uk/

1. yum update 

2.yum localinstall preupgrade-assistant-1.0.2-36.0.1.el6.centos.x86_64.rpm

3. 
		

	


		

	