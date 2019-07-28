title: 'Kubernetes 组件和架构介绍'
date: 2019-07-29 22:36:31
tags: [云原生, Cloud, Container]
---

# Kubernetes 组件和架构介绍

## Kubernetes 组件

一个 Kubernetes(k8s) 集群的基本构成采用的主从方式，包含 Master Node（主节点） 和 Worker Node（从节点） 

### Master 节点

主节点属于 K8s 的控制平面，掌管着集群的全局调度，集群的事件的检测和处理等。主节点内部包含若干组件，尽管每个组件本身可以在集群中任意一台机器上运行，启动脚本为了简化，将组件部署在同样的一台机器上，为了保证集群的高可用，集群环境一般包括多个主节点。下面简单梳理一下主节点的各个组件构成：

- kube-apiserver

  kuber-apiserver 是 k8s 控制平面的前端，对外提供 API 服务，可以方便的水平扩展

- etcd

  为 k8s 集群数据的提供一致性高可用 K, V 存储

- kube-scheduler

  负责将 pod 调度到合适的 Worker Node 上，调度综合考虑，硬件，软件，策略限制，亲和性设置，数据本地化，不同负载之间的干扰等

- kube-controller-manager

  控制器主要是主节点对于从节点管理，服务层面的鉴权，服务端点维护等，包括：

  - Node Controller：负责从节点失效的监测和处理
  - Replication Controller： 维护 Pod 服务数量的一致性
  - Endpoints Controller： 服务端点的变化相关的维护
  - Service Account & Token Controllers： 名字空间下的账户和 API 访问口令的管理

- cloud-controller-manager

  k8s 和不同云服务厂商对接的管理器，主要包含不同云厂商的定制化相关的功能管理，社区希望云厂商相关的代码和 k8s 核心代码尽量减少依赖，各自独立开发，依靠 cloud-controller-manager 的链接方式运行云厂商相关的管理器，但是历史原因还是有一些相关的控制器依赖，主要包括：

  - Node Controller： 节点停止服务后，节点是否在云厂商那边真正删除
  - Route Controller： 路由的建立
  - Service Controller： 云负载均衡器
  - Volume Controller： 卷存储的生命周期交互

### Worker 节点

Worker 节点主要是提供 k8s 的容器运行环境，维护运行的 pods，从节点组件包括：

- kubelet

  从节点上运行 Agent，保证 pod 中容器正常运行，健康检查管理等

- kube-proxy

  从节点运行的网络代理，提供路由转发

- container runtime

  容器运行环境，支持 docker，containerd， cri-o，rktlet，只要是符合 CRI 接口要求的运行环境都支持。

### 其他扩展

扩展插件利用 k8s 资源等机制实现了集群相关的一些特性功能，主要包括：

- 提供网络通信和网络策略管理

- 服务发现（DNS）

- 可视化和管理 Web 平台

- 集群内部容器资源的监控

- 集群级别的日志处理

  

## Kubernetes 架构

来自 Kubernetes 官方的文档中给出 k8s 和云控制管理器演化的架构对比，其中也包含了关于不同组件的架构组成：

{% asset_img pre-ccm-arch.png %}

{% asset_img post-ccm-arch.png %}

## Kubernetes 高可用

k8s 的高可用多主节点架构如下：

{% asset_img ha-master-gce.png %}

除了上面这种堆叠式架构外，还可以采用另外一种外部的 etcd 节点架构，

{% asset_img kubeadm-ha-topology-external-etcd.svg %}

两种架构各有优缺点，根据自身的需求进行选择。

## Kubernetes Service 网络架构

在 k8s 中， service 这个对象代表一个提供网络服务的应用，这个应用背后就是一组 pods。  pods 在每个从节点上运行，利用 kube-proxy 进行相应的网络转发和代理，kube-proxy 的实现方式是虚拟 IP 的方式，具体有以下几种形式：

- 用户空间代理模式 （默认是 round-robin）
  
  {% asset_img services-userspace-overview.svg %}

- iptables 代理模式 （默认是 random）
  
  {% asset_img services-iptables-overview.svg %}

- ipvs 代理模式 （支持多种，rr，lc，dh，sh, sed, nq)

  {% asset_img services-ipvs-overview.svg %}

  k8s  主要支持两类服务发现模型，环境变量和DNS

  k8s 中服务对外的发布主要有四类方式：

  - ClusterIP （集群内部 IP）

  - NodePort (每个节点 IP， Port)

  - LoadBalancer（云厂商的负载均衡器）

  - ExternalName（CNAME 方式，无需代理方式）

    

## Kubernetes 生产环境部署

k8s 集群按照相应层次的面向对象不同，涵盖应用，数据面，控制面，集群基础设施，常规集群运维等方面，究竟是自身管理还是使用云产品打包方案，有如下几种形式：

{% asset_img kubernetessolutions.svg %}

对于上面的几种解决方案，各个云厂商支持的粒度有很大差别，具体可以见参考资料中表格总结

## 参考资料

- https://kubernetes.io/docs/concepts/cluster-administration/addons/
- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/architecture/cloud-controller/https://kubernetes.io/docs/concepts/architecture/cloud-controller/
- https://kubernetes.io/docs/tasks/administer-cluster/highly-available-master/
- https://kubernetes.io/docs/concepts/services-networking/service/
- https://kubernetes.io/docs/setup/#production-environment
