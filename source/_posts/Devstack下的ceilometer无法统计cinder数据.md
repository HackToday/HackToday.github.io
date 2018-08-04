title: 'Devstack下的ceilometer无法统计cinder数据'
date: 2013-05-07 22:13:25
tags: 云计算
---

前言解释：

Devstack：快捷搭建openstack开发环境的工具
ceilometer： Openstack下的一个project，用于统计各种资源，比如instance，image等的使用情况。

问题：

启动ceilometer后会发现相关的cinder的部分没有统计。

原因：

devstack关于cinder的配置部分和ceilometer在notification相关的的exchange name的配置不同。
在cinder.conf中，

```
control_exchange=openstack
```

而在ceilometer的ceilometer.conf中，默认的是

```
cinder_control_exchange=cinder
```

devstack没有对这里进行特殊配置，两个exchange不一致，当然cinder的notification就不会被收到。 ceilometer就无法更新cinder使用的情况。


解决方法：

修改cinder.conf中的exchange name为cinder，重新启动cinder相关的服务，即可。
