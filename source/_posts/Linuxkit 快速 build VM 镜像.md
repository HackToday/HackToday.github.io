title: 'Linuxkit 快速 build VM 镜像'
date: 2017-05-19 07:50:49
tags: 云计算
---

Linuxkit 是什么？ 这个我原来这博文【1】中大概介绍过，今天再重新说一下，其实 Linuxkit 就是一个强大的工具集，这个工具集可以定制化基于容器的操作系统，这些镜像可以部署到现有的一些云平台上，因为本身支持很多的镜像格式 kernel+initrd，qcow2 等。

废话不多说，我们演示一下如何 build 一个镜像，拿 docker 为例

1> $ git clone https://github.com/linuxkit/linuxkit.git
2> $ cd linuxkit
运行 make 命令

一会就 build 完成对应的工具，包括 linuxkit，moby，rtf

linuxkit 命令主要用于和 cloud 或者本地 hypervisor 交互的： push 镜像，部署镜像
moby 用于 build VM 镜像的
rtf 社区开发中运行集成测试的

3> 使用上面的工具开始 build 镜像

$ cd examples
$ moby build docker
默认生成 kernel+initrd 镜像，还有对应的启动参数

```
-rw-r--r-- 1 root root 71622481 May 17 11:42 docker-initrd.img
-rw-r--r-- 1 root root  7541328 May 17 11:42 docker-kernel
-rw-r--r-- 1 root root       40 May 17 11:42 docker-cmdline
```

$ 运行 docker 镜像

```
$ linuxkit run docker
```

注意这里运行的时候，依赖一些可支持的平台，如下：

```
Supported backends are (default platform in brackets):
  gcp
  hyperkit [macOS]
  qemu [linux]
  vmware
  packet
```

本文一台 CentOS 机器，装了相关的 QEMU 相关的软件，否则会出现不能运行，或者进入VM 后，出现乱码的状况

最后有如下的画面

```
[   12.764709] tsc: Refined TSC clocksource calibration: 2294.665 MHz
[   12.764991] clocksource: tsc: mask: 0xffffffffffffffff max_cycles: 0x2113854cd53, max_idle_ns: 440795215552 ns
[   12.773755] Freeing unused kernel memory: 820K (ffff8ba191933000 - ffff8ba191a00000)
[   12.823433] Freeing unused kernel memory: 1012K (ffff8ba191d03000 - ffff8ba191e00000)
[   13.782833] clocksource: Switched to clocksource tsc

Welcome to LinuxKit

                        ##         .
                  ## ## ##        ==
               ## ## ## ## ##    ===
           /"""""""""""""""""\___/ ===
      ~~~ {~~ ~~~~ ~~~ ~~~~ ~~~ ~ /  ===- ~~~
           \______ o           __/
             \    \         __/
              \____\_______/


/ #
```

执行 runc list 查看

```
/ # runc list
ID          PID         STATUS      BUNDLE                         CREATED                          OWNER
dhcpcd      607         running     /run/containerd/linux/dhcpcd   2017-05-18T10:36:44.584310687Z   root
docker      639         running     /run/containerd/linux/docker   2017-05-18T10:36:47.003939228Z   root
ntpd        668         running     /run/containerd/linux/ntpd     2017-05-18T10:36:48.825102369Z   root
```

进入 docker 容器，

```
/ # runc exec -t docker sh
/ # docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
/ #
```

这样一个 VM 中的容器中运行的 docker 进程就 OK 了，也很简单

当然了社区这个工具开发相对时间不长，所以有些调试或者使用易用性上还有很多要做的工作

参考文献：

1. http://blog.chinaunix.net/uid-21335514-id-5763176.html
2. https://github.com/linuxkit/linuxkit
