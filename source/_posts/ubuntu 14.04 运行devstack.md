title: 'ubuntu 14.04 运行devstack'
date: 2014-05-16 16:53:47
tags: 云计算
---

前段时间的ubuntu的14.04的发布，可谓一直就想试试新的用户体验，软件工程师要有尝试新鲜事物的精神是吧，不是“神精”

下载iso，挂载virtualbox的新创建的虚拟机上，

1. 下载代码

```
git clone https://github.com/openstack-dev/devstack.git
```

这个是master的branch就是针对openstack Juno的分支

2. 配置 （根据自己的环境配置相应的***地方）

```
[[local|localrc]]
ENABLED_SERVICES="$ENABLED_SERVICES,-rabbit,-zeromq,qpid"
ENABLED_SERVICES="$ENABLED_SERVICES,-n-net,q-svc,q-agt,q-dhcp,q-l3,q-meta,q-metering,neutron,tempest"
ADMIN_PASSWORD=***
MYSQL_PASSWORD=***
SERVICE_PASSWORD=$ADMIN_PASSWORD
LOGFILE=$DEST/logs/stack.sh.log
HOST_IP=*****
KEYSTONE_CATALOG_BACKEND=sql
SERVICE_TOKEN=***

QPID_USERNAME=***
QPID_PASSWORD=***

FLOATING_RANGE=***
FIXED_RANGE=***
FIXED_NETWORK_SIZE=**
FLAT_INTERFACE=eth1
NETWORK_GATEWAY=****
PUBLIC_NETWORK_GATEWAY=***
```

3. 运行

```
./stack.sh
```

4. 执行完毕，openstack的一切服务运行良好

虽然devstack的主页没写支持ubuntu 14.04，但细细想想ubuntu多年前押宝到openstack，作为社区广泛流行的支持OS LTS的版本自然要相当重视，
应该是ubuntu团队对devstack贡献了不少代码，保证发布的14.04完美支持吧
