title: 借助docker log调试docker 启动timeout问题
date: 2015-11-14 12:03:25
tags: 云计算
---


						我们在使用docker的过程，经常有需求要调试docker的问题，所以需要了解对应log的目录:

* Ubuntu - /var/log/upstart/docker.log
* Boot2Docker - /var/log/docker.log
* Debian GNU/Linux - /var/log/daemon.log
* CentOS - /var/log/daemon.log | grep docker
* Fedora - journalctl -u docker.service
* Red Hat Enterprise Linux Server - /var/log/messages | grep docker  

比如前几天调试docker的时候，发现docker的服务总是启动超时，
 docker.service operation timed out. Terminating.
根据log仔细排查后，发现原来docker启动过程，加入的docker-storage在初始化过程中(第一次启动过程)会比较慢， 然后修改了systemd docker服务的配置

[Service]
TimeoutStartSec=0

配置TimeoutStartSec关闭timeout限制，这样docker服务就能正常启动了。所用的时间计算了一下，发现是3~4mins，估计是因为嵌套虚拟机再启动docker的性能问题，如果裸机上的虚拟机启动docker 似乎很少遇到这种情况。

参考:
http://stackoverflow.com/questions/30969435/where-is-the-docker-daemon-log
http://container-solutions.com/running-docker-containers-with-systemd/                                   