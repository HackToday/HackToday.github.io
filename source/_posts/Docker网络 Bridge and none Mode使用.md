title: Docker网络 Bridge and none Mode使用
date: 2016-05-28 23:00:08
tags: 云计算
---


	Docker默认的是三种网络类型，bridge，none，host

	其中bridge是典型的类似VM的linux-bridge网络

	

	具体在每次运行docker run的时候，container都会从bridge对应的网络范围中选择一个空闲的ip，然后将其设置为对应容器的eth0的ip。Docker bridge的网络ip设置对应每个容器的默认网关，提供给容器来访问外部的网络。

	

	因为docker自己操作的容器网络名字空间并不是放在/var/run/netns中，所以ip netns无法直接查看，可以通过以下的方式来访问：

	

	下面的过程是对none类型的容器进行网络配置的例子：

