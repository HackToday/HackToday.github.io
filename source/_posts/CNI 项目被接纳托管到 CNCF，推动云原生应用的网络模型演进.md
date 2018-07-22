title: CNI 项目被接纳托管到 CNCF，推动云原生应用的网络模型演进
date: 2017-05-24 08:24:32
tags: 云计算
---


	今天 CNCF 又迎来了一位重要网络项目成员——CNI，这也是放在 CNCF 托管的第10个项目

	

	CNI 是一个关于容器网络接口标准的项目，由 CoreOS 发起，Redhat OpenShift, Apache Mesos, Cloud Foundry, Kubernetes, Kurma 和 rkt 公司创立。

	CNI 定义了一套通用接口，接口面向网络插件和容器运行环境，CNI 定义的接口也是最小化，主要关注：容器的网络连通性和容器移除的时候的相关网络资源释放。

	

	CNI 主要包含了三大组件 （参考下图）

	

	1. CNI Specification： 定义了容器运行时和网络插件之间的API，实现容器网络的建立和销毁

	2. Plugins：支持不同网络技术和方案的扩展性手段

	3. Library：提供了 CNI Specification 一种 Go 实现，容器运行时可以很容易的使用 CNI

	参考：

	 
