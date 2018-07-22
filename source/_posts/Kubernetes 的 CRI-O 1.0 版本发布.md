title: Kubernetes 的 CRI-O 1.0 版本发布
date: 2017-10-18 08:44:06
tags: 云计算
---


						CRI-O 的源头：

Kubernetes 作为编排领域的巨头，一直也是在标准化兼容的路上一路前行，为了支持多种的容器运行时系统（Docker, Rkt) 等，减少集成维护的成本，开发了自身的 CRI 接口规范，由于事实上的 OCI 已经被多方厂商认可，所以 CRI 和 OCI 的桥接也是自然的事情，CRI-O 这个项目来源也就是如此。


CRI-O 的内部窥探：（下面的图片来源于 https://www.redhat.com/en/blog/introducing-cri-o-10）


CRI-O 项目也特别声明了自身的定位： 只是用于 Kubernetes 用来管理和运行 OCI 标准兼容的容器，也不是面向开发人员的，或者用来打包镜像的。

差不多另外一天， docker 官方也公布了自身的对于 kubernetes 的编排的原生支持，Docker 社区版和企业版都可以使用 docker 官方的类似 compose 工具来部署维护 Kubernetes 的pods 和 services 等。


参考文献：
1. https://blog.docker.com/2017/10/kubernetes-docker-platform-and-moby-project/
2. https://www.redhat.com/en/blog/introducing-cri-o-10                                   