title: 'Redhat 6.5 升级 Redhat 7的经历-也是醉了'
date: 2014-12-17 23:06:01
tags: LINUX
---

1. https://access.redhat.com/solutions/21964
   Redhat 4,5,6之间的upgrade，官方不给与支持，推荐使用fresh install，然后把对应软件的配置和数据迁移到新的server上

2. Start from Redhat 7,
   Redhat 对一些特定的case给与了支持，但是支持的力度有限 https://access.redhat.com/node/637583/

3. 在Redhat Summit上
   http://rhsummit.files.wordpress.com/2014/04/cantrell_w_1650_migrating_and_upgrading_rhel.pdf

   Redhat官方的升级资料有个可恶的问题就是，给出的升级包没有下载的连接，不知道是不是需要什么subscription number 才能看到？
   所以就索性按照Damian Zaremba写的关于 centos 6.5升级到7的步骤执行： http://damianzaremba.co.uk/

```
	1. yum update 

	2. yum localinstall preupgrade-assistant-1.0.2-36.0.1.el6.centos.x86_64.rpm

	3. yum localinstall preupgrade-assistant-contents-0.5.14-1.el6.centos.noarch.rpm

	4. yum localinstall redhat-upgrade-tool-0.7.22-3.el6.centos.noarch.rpm

	5. 
	[root@testnode-***** ~]# redhat-upgrade-tool --iso=RHEL-7.0-20140507.0-Server-x86_64-dvd1.iso  --force
	setting up repos...
	getting boot images...
	vmlinuz-redhat-upgrade-tool                                                                  | 4.7MB     00:00 ...
	initramfs-redhat-upgrade-tool.img                                                            |  32MB     00:00 ...
	setting up update...
	upgradeiso/filelists_db                                                                      | 3.0 MB     00:00 ...
	finding updates 100% [===========================================================================================]
	testing upgrade transaction
	rpm transaction 100% [===========================================================================================]
	rpm install 100% [===============================================================================================]
	setting up system for upgrade
	Finished. Reboot to start upgrade.

	6. Reboot
```

悲剧的是重启后，发现系统根本起不来，不能load image.
因为原来系统做过快照，所以恢复一次，重试还是出现同样的问题，以亲身经历验证这个依靠centos的包升级不靠谱。
除非Redhat官方大发慈悲，公布相应的下载包，要不我也是对其升级方案持有悲观态度。
