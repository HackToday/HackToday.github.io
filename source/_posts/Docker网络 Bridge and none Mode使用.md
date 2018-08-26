title: 'Docker网络 Bridge and none Mode使用'
date: 2016-05-28 23:00:08
tags: 云计算
---

Docker默认的是三种网络类型，bridge，none，host
其中bridge是典型的类似VM的linux-bridge网络

具体在每次运行docker run的时候，container都会从bridge对应的网络范围中选择一个空闲的ip，然后将其设置为对应容器的eth0的ip。
Docker bridge的网络ip设置对应每个容器的默认网关，提供给容器来访问外部的网络。

因为docker自己操作的容器网络名字空间并不是放在/var/run/netns中，所以ip netns无法直接查看，可以通过以下的方式来访问：

下面的过程是对none类型的容器进行网络配置的例子：

```
$ docker inspect -f '{{.State.Pid}}' 63f36fc01b5f
2778
$ pid=2778
$ sudo mkdir -p /var/run/netns
$ sudo ln -s /proc/$pid/ns/net /var/run/netns/$pid

$ ip addr show docker0
21: docker0: ...
inet 172.17.42.1/16 scope global docker0
...

# Create a pair of "peer" interfaces A and B,
# bind the A end to the bridge, and bring it up

$ sudo ip link add A type veth peer name B
$ sudo brctl addif docker0 A
$ sudo ip link set A up

# Place B inside the container's network namespace,
# rename to eth0, and activate it with a free IP

$ sudo ip link set B netns $pid
$ sudo ip netns exec $pid ip link set dev B name eth0
$ sudo ip netns exec $pid ip link set eth0 address 12:34:56:78:9a:bc
$ sudo ip netns exec $pid ip link set eth0 up
$ sudo ip netns exec $pid ip addr add 172.17.42.99/16 dev eth0
$ sudo ip netns exec $pid ip route add default via 172.17.42.1
```