title: '大话容器标准-OCI CNCF'
date: 2017-10-29 14:22:37
tags: 云计算
---

这是以前写的文章，一直没有分享出来，一方面觉得业界还在不停的变化，另一方面还在忙别的事情，随着前些日子，docker 官方原生支持 Kubernetes 的部署，我觉得回过头来看看以前自己写的这篇文章还是很有意思的事情。

OCI 2015年6月建议，发起者主要是 docker 公司还有容器行业的其他的公司。最初主要是解决容器运行标准化的问题，后来扩展包括了对于容器镜像标准的问题的涵盖。

目前主要包含以下标准：
http://www.github.com/opencontainers/runtime-spec
http://www.github.com/opencontainers/image-spec

主要的流程是，从镜像源下载 OCI 镜像，解压符合 OCI 运行时的文件系统 Bundle，然后由 OCI Runtime 运行（例如runc）
不难看出，上面 OCI的使命主要是解决底层容器运行相关容器打包标准化和运行环境规范，实现容器镜像的跨云，平台的即插即用。

第一章. 群雄逐鹿

 - Containerd
docker 公司官方实现的符合 OCI 运行规范的实现是 runc，并且捐赠给 OCI 组织。

对于 runc 这么底层的运行程序，显示对于更高一级管理（host级别，或者cluster 级别）是需要一定程度的抽象和封装，为此就有了所谓的 containerd和随着一套的工具集合，这些也是目前 docker 版本中被集成和实现了，具体的工具交互流程图如下：



具体的内部架构图如下： 



用户或者外部系统通过 API ，来跟子系统进行交互，包括，镜像相关（distribution 拉取镜像，Bundle 提取和打包镜像），容器相关（bundle 的执行，来进行容器的创建运行）
对应下面的组件，包括：Executor（容器运行时的实现），Supervisor（容器状态的监控和上报），Metadata（镜像和bundle持久化引用的存储，存储到graph db中），Content（镜像RO部分），Snapshot（graphdriver 部分，对于容器镜像部分的管理），Events（事件支持），Metrics（指标）。

程序内部流程图：


- Rkt

Rkt 和 Docker 走的架构有些不同，走的是 daemon less，以pod为基础单位，下面的是 rkt 的一个架构图：



stage0: 根据不同的触发方式，调用 rkt， 然后获取 ACL 镜像，生成 pod 相关的元数据，创建相关的文件系统，解压镜像到相关的目录。
stage1: 根据stage0产生的镜像和pod元数据，创建相应的隔离环境，（支持的flavor 有： fly,systemd,kvm）
stage2: 在 stage1的隔离环境中，调用对应的app 可执行程序

相比 docker，这个本身不存在单点故障，rkt 本是主要接口是一个命令行工具，不需要类似 docker dameon 或者 containerd 类似的支撑。
不过 docker 从原来的 docker daemon 重构迁移到模块化结构的 containerd 方案后，通过 live-restore 是可以实现 docker daemonless 的
container，重启 docker daemon ，容器仍然保持运行。

参看 coreos 网站给了一个 关于 rkt vs docker 比较，其实这两个现在不适合直接比较了，更应该是比较 rkt 和 containerd 之间的比较。





不论是 containerd 和 rkt，都在其中对于 OCI 的支持列入到了自身的 Roadmap 中，具体分别是：
https://github.com/containerd/containerd/blob/master/ROADMAP.md
https://github.com/rkt/rkt/blob/master/ROADMAP.md

值得一提的是，关于 rkt 是 CoreOS 主导的，CoreOS 公司有些融资是来自于谷歌风投的。
（想想rkt 现在努力支持k8s CRI， k8s 中使用的 etcd，天生的pod 适配，等待..）

CNCF

容器本身的特点，极大的促使新型的应用转交和发布，服务的动态关联和发现。一些互联网企业开始从传统的单体架构转向微服务架构，依次衍生出对于云原生应用的需求和技术，CNCF 就是在这种背景下产生，主要是目标是提供可使用的软件栈满足以下需求：提供服务容器化、动态编排、面向微服务的。

说道这个 CNCF 就不得不说 google 和 kubernetes，这个基金会成立之初也是google 为了推广 kubernetes 生态的发展，kubernetes 作为容器管理系统，是一个面向企业级的偏重的全能型实现方案，几乎所有的容器服务的场景和融合方案（网络，存储，编排，AutoScale 等）都会涵盖。

CNCF 基金会以面向可用系统级别为出发点，吸纳了其他衍生项目的加入，包括但不限于：



CNCF 的摊子铺的可算比较大， 



除了 SDN， SDS， IaaS， OS， Container Runtime 之外。上面的部分，还有接口的支持和集成，都包括在内。

第二章  部落联盟

关于 OCI 和 CNCF 还有一个比较有趣的双方协定，

OCI 中，说
How does OCI integrate with CNCF?
A container runtime is just one component of the cloud native technical architecture but the container runtime itself is out of initial scope of CNCF (as a CNCF project), see the charter Schedule A for more information. 

CNCF 这样说
For further discussion
The following are considered out of scope for now, but might need to be included later.

    * Application environment. Crafting a well formed, minimal userland environment for cloud native applications.
    * OCI topics (container format and runtime). The goal is to integrate with the OCI group.

虽然这里看到还是有井水河水之分，但是依据微妙的声明，可以看到还是会有融合的可能。
CNCF 的影响力不断增大，docker 官方也打算并且提交将containerd 捐献给CNCF， CoreOS 也是同样提交了 rkt 给 CNCF，希望将容器运行核心项目交由 CNCF 基金会维护，更加发展壮大。这可谓树大好乘凉。

那我们看看containerd 和 rkt 在 未来的云原生系统中占据什么位置，如下：



显然，无论是 docker 还是 rkt 都希望自身在未来的容器生态系统占据核心重要一环。
而且未来，docker 不满足 containerd 只是简单的容器运行和进程管理，他希望可以扩充更多的，涵盖distribution，storage，和networking，下面两张图对比来看：



第三章 联盟求同存异

CNCF 在 Berblin KubeCon 上宣布同时接受了 containerd and rkt 这两个 runtime 项目 作为 CNCF 孵化项目
https://thenewstack.io/separate-votes-cncf-adopts-dockers-containerd-coreos-rkt/

所以 CNCF 官方网站上就有了以下的有趣的成员图：



第四章 生态之争

每年的DockerCon 都会不甘寂寞，放出一些很酷的 feature 和声明，这次 DockerCon 2017 也不例外， Docker 公司做出来两个重要的宣布

Docker 开源社区单独命名： Docker 成为公司商业产品的代名词，开源社区的运营以 Moby 为代名词，Moby 代表社区的围绕容器系统生态相关的各个组件，框架项目统称，基于这些开源化的组件，不同的系统集成或者公司都可以开发自身的容器管理集成系统，当然也包括 Docker 公司自己。Docker 公司本身的很多产品（比如Docker DataCenter）是基于这些组件搭建的，同时包含了一些特别的高级功能，这样 Docker 名字命名的改变，应该是公司自身一来明确的产品界限，二来希望基于自身的技术的开源生态系统更加更加丰富（看看人家 kubernetes 生态系统搞得大不大），这也是一场不确定的生态系统战争，双方胜负未定，至少对方都希望不要输的太早。

参考资料： 
1.  https://www.opencontainers.org/
2.  https://github.com/docker/containerd/blob/master/design/architecture.md
3.  https://coreos.com/rkt/docs/latest/devel/architecture.html
4.  https://coreos.com/rkt/docs/latest/rkt-vs-other-projects.html#rkt-vs-runc
5.  https://www.cncf.io/about/charter/
6.  https://www.opencontainers.org/faq
7.  https://containerd.io/
8.  https://blog.docker.com/2017/03/docker-donates-containerd-to-cncf/
9.  https://coreos.com/blog/rkt-container-runtime-to-the-cncf.html
10. http://events.linuxfoundation.org/sites/events/files/slides/(OSF_Mr.%20Chris%20Aniszczyk)CNCF%20(OS%20Forum%20Japan%202016).pdf
11. https://github.com/moby/moby/pull/24970
