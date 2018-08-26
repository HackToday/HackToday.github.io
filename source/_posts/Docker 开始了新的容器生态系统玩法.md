title: 'Docker 开始了新的容器生态系统玩法'
date: 2017-04-19 08:53:17
tags: 云计算
---

这两天在Austin, Texas.举办的 DockerCon 2017 释放了重要的信息，Docker 官方发布了两个开源的项目，一个是 LinuxKit ，一个是 Moby

LinuxKit 目前看应该是为容器系统搭建基础的可定制化，可裁剪的 Linux 发行版，这个是为了解决目前容器系统运行的一些基础镜像过分笨重的问题

Moby 更是 Docker 官方升级容器系统的一个大玩法，不再局限于单纯的 Swarm 或者 Docker 运行时了，而是面向系统集成厂商或者创新实现的 Hacker们，开创的一种 Framework + Library 方法，可以利用广泛的开源的容器相关的组件（比如 runtime，build，image，volume 等）搭建出个定制化的容器化大系统，这种策略可以初期看出来更像平台玩法，吸引更多的集成商进来助力Docker 相关容器系统的推广和使用。当然这个项目不是针对应用开发者的，毕竟应用开发者不关心底层的系统构建。

通过这两个重要信息，我们可以看出以前的工具类的玩法逐渐变为平台玩法，平台开发吸引更多的厂商加入，才可能把市场影响力扩大。为了支持这个平台玩法，从基础镜像裁剪到运行时，到build，security 等组件化，为新的玩法提供了这种可能。

或无疑问，这种开放策略必将容器生态扩大，但这种策略能否助力 Docker 公司自身的成功，还是需要一些生态推进和执行力保证的。让我们拭目以待。

来源：

https://blog.docker.com/2017/04/introducing-the-moby-project/
http://www.eweek.com/cloud/docker-opens-ups-container-platform-with-linuxkit-and-moby-project                                   