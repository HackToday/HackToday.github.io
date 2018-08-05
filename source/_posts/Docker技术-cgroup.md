title: 'Docker技术-cgroup'
date: 2014-10-13 08:50:42
tags: 虚拟化
---

Docker容器采用了linux内核中的cgroup技术来实现container的资源的隔离和控制。
关于cgroup我们需要了解的它的知识点：

1. 基本概念

cgroup涉及到几个概念如下：
cgroup：以某种方式，将某些任务和subsystem进行关联
subsystem：基于cgroup的资源管理器，可以看作一种资源（比如CPU，Memory， I/O等），实现对一个cgroup的task的调度和控制
hierarchy：对crgoups和subsystems以某种形式进行的组织，cgroup组织形式是树结构，subsystem会关联连接到hierarchy上。

上面的概念有点抽象，看个简单的例子就明白了，比如redhat官网的一个图：

上面的就是一个1个hierarchy，有两个subsystem（cpu和memory） attach上，其中的cgroups以tree的结构组织。

既然cgroup是什么以及结构弄清楚了，那到底cgroup如何实现的资源的管理控制，cgroup是和task（乜可以看作是系统中的进程）关联的，这样不同的cgroup在subsystem中的资源控制下就能够分配到不同的份额或者设定不同的限制，同时可以统计监控资源的消耗情况。

2. 如何使用Cgroups
  如果我们最原始的使用cgroup，不借助高级的docker方式，那么具体步骤如下：

```
To start a new job that is to be contained within a cgroup, using
the "cpuset" cgroup subsystem, the steps are something like:

 1) mount -t tmpfs cgroup_root /sys/fs/cgroup
 2) mkdir /sys/fs/cgroup/cpuset
 3) mount -t cgroup -ocpuset cpuset /sys/fs/cgroup/cpuset
 4) Create the new cgroup by doing mkdir's and write's (or echo's) in
    the /sys/fs/cgroup virtual file system.
 5) Start a task that will be the "founding father" of the new job.
 6) Attach that task to the new cgroup by writing its PID to the
    /sys/fs/cgroup/cpuset/tasks file for that cgroup.
 7) fork, exec or clone the job tasks from this founding father task.

For example, the following sequence of commands will setup a cgroup
named "Charlie", containing just CPUs 2 and 3, and Memory Node 1,
and then start a subshell 'sh' in that cgroup:

  mount -t tmpfs cgroup_root /sys/fs/cgroup
  mkdir /sys/fs/cgroup/cpuset
  mount -t cgroup cpuset -ocpuset /sys/fs/cgroup/cpuset
  cd /sys/fs/cgroup/cpuset
  mkdir Charlie
  cd Charlie
  /bin/echo 2-3 > cpuset.cpus
  /bin/echo 1 > cpuset.mems
  /bin/echo $$ > tasks
  sh
  # The subshell 'sh' is now running in cgroup Charlie
  # The next line should display '/Charlie'
  cat /proc/self/cgroup
```

3. 使用Docker，减少冗长细节的cgroup使用
  虽然Docker给我们带来了极大的方便，但是我们还是需要了解一些cgroup的操作使用，这样才更加有信心处理各种问题，

3.1 使用cgroup-bin
 一般操作系统没有默认安装这个工具，我们自己需要安装，提供了很多有趣而且很棒的命令：

```
 lssubsys 列出包含subsystem的hierarchy
 lscgroup 列出所有的cgroup

 /proc/cgroups 列出了系统中cgroup的subsystem
 /proc//cgroup  列出进程所属的cgroup，包含了cgroup的
```

3.2 Docker的结合
比如我们docker run了一个container，
 
先找出contianer的ID，

```
   docker ps --no-trunc
```

根据上面找出的ID，找到对应的tasks ID，

```
   cat  /sys/fs/cgroup/cpu/docker//tasks
```

查看对应的cgroup

```
   cat /proc//cgroup
```

4. 不同操作系统暴露的subsystem的区别
 ubuntu就和redhat有所不同，redhat有10中cgroup subsystem
 如果默认安装的ubuntu，比如14.04， 你会发现，没有net_cls和net_prio，具体如何搞出来，参看这个帖子

参考资料：

1.  Linux Kernel Doc about Cgroups
   https://www.kernel.org/doc/Documentation/cgroups/cgroups.txt
2. Cannot find network subsystem in cgroup on Ubuntu 12.04 LTS
   http://serverfault.com/questions/485919/cannot-find-network-subsystem-in-cgroup-on-ubuntu-12-04-lts
3. Redhat cgroup 
    https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Resource_Management_Guide/ch01.html
4. Docker cgroup
    https://docs.docker.com/articles/runmetrics/#control-groups
