title: Docker技术-cgroup
date: 2014-10-13 08:50:42
tags: 虚拟化
---


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

	

	

	3. 使用Docker，减少冗长细节的cgroup使用

	  虽然Docker给我们带来了极大的方便，但是我们还是需要了解一些cgroup的操作使用，这样才更加有信心处理各种问题，

	3.1  使用cgroup-bin

	 一般操作系统没有默认安装这个工具，我们自己需要安装，提供了很多有趣而且很棒的命令：

	

	 lssubsys 列出包含subsystem的hierarchy

	 lscgroup 列出所有的cgroup

	

	 /proc/cgroups 列出了系统中cgroup的subsystem

	 /proc/
	

	3.2. Docker的结合

	比如我们docker run了一个container，

	 

	 先找出contianer的ID， 

	   docker ps --no-trunc

	

	 根据上面找出的ID，找到对应的tasks ID，

	   cat  /sys/fs/cgroup/cpu/docker/
	  

	 查看对应的cgroup

	   cat /proc/
	

	4. 不同操作系统暴露的subsystem的区别

	 ubuntu就和redhat有所不同，redhat有10中cgroup subsystem

	 如果默认安装的ubuntu，比如14.04， 你会发现，没有net_cls和net_prio，具体如何搞出来，参看这个帖子

	

	

	参考资料：

	1.  Linux Kernel Doc about Cgroups

	   

	2. Cannot find network subsystem in cgroup on Ubuntu 12.04 LTS

	   

	3. Redhat cgroup 

	    

	4. Docker cgroup

	    

	
