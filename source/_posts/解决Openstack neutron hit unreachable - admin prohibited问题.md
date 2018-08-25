title: '解决Openstack neutron hit unreachable - admin prohibited问题'
date: 2015-11-13 23:38:02
tags: 云计算
---

使用了Redhat 7.1 搭建的devstack环境发现boot的instance无法ping通主机的public ip，后来通过抓包发现原来是类似的一个问题:

https://lists.launchpad.net/openstack/msg25392.html

只需要把防火墙下面的规则去掉就行了

```
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
```