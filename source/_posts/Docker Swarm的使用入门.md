title: 'Docker Swarm的使用入门'
date: 2015-04-23 15:04:26
tags: 云计算
---

Swarm是docker官方的native cluster方案，实现将Docker的host pool虚拟为一个主机，兼容docker的标准API， 保证了所有可以和docker daemon通信交互的软件，可以无缝的移植和docker swarm集群交互。

Docker的官方文档中提供了多种方式来搭建swarm nodes

1. 官方例子使用docker hub token发现服务

2. 静态文件方式

3. etcd方式

4. consul方式

5. zookeeper方式

6. 静态ip列表方式

7. ip范围模式匹配方式

本文选1，2 为例介绍

实验环境，
搭建两个机器，实体机或者虚拟机都可以，保证可以和外网通信。

方式1.
1.1创建cluster id

```
docker run --rm swarm create
```

1.2 在每个节点运行 docker daemon，需要以tcp端口监听方式运行

```
docker -H tcp://0.0.0.0:2375 -d
```

在cluster中注册每个节点的ip

```
docker run -d swarm join --addr= token://
```

1.3 启动swarm manager

```
docker run -d -p :2375 swarm manage token://
```

1.4 检查集群状态
   docker -H tcp:// info

   检查集群节点信息
   docker run --rm swarm list token://

如下：

```
Containers: 40
Strategy: spread
Filters: affinity, health, constraint, port, dependency
Nodes: 2
host1: node1:2375
  ?.Containers: 32
  ?.Reserved CPUs: 0 / 1
  ?.Reserved Memory: 0 B / 4.054 GiB
host2: node2:2375
  ?.Containers: 8
  ?.Reserved CPUs: 0 / 1
  ?.Reserved Memory: 0 B / 4.054 GiB
 1.5 使用集群，就是针对swarm manager 来操作
      docker -H tcp:// run ...
```

方式2：

手工维护，有点麻烦，但是不依赖docker hub外部服务
这里的主要区别是：
 
2.1  添加节点信息，

```
$ echo >> /tmp/my_cluster
$ echo >> /tmp/my_cluster
```

2.2 在每个节点运行 docker daemon， 同1.2

2.3 启动swarm manager

```
docker run  -it -v /tmp/:/mywork -p 6000:2375  swarm manage file:///mywork/my_cluster
```

2.4. 检查集群状态

```
docker -H tcp:// info
```   

检查节点信息

```
docker run  -v /tmp/:/mywork  --rm  swarm list file:///mywork/my_cluster
```

2.5  操作集群，同1.5


参考资料：

1. http://docs.docker.com/swarm/
2. https://docs.docker.com/swarm/discovery/