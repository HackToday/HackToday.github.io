title: '如何登录到Docker的container中'
date: 2014-10-04 12:24:30
tags: 虚拟化
---

使用Docker部署container后，我们总有类似的需求：登录到container中进行一些操作。

常见的方式
1. 有ssh方式，特点是不需要特别的root权限，但是container需要安装sshd
2. 使用nsenter来从container获得一个shell实现登录
3. 使用nsinit

本文主要介绍nsenter的使用

nsenter使用非常方便，但是有的操作系统发行版本util-linux包比较老，所以没有包含这个nsenter，
那么你需要自己编译和安装，对于hacker们来说，源码编译安装不是小case嘛，走起！

注明： 下面的命令运行以`Ubuntu 14.04`为例

1）下载源码

```
git clone git://git.kernel.org/pub/scm/utils/util-linux/util-linux.git util-linux
cd util-linux/
```

2）安装依赖包（这个具体缺少的情况，会在运行 `./autogen.sh`的提示，你也可以直接运行3），根据提示来安装对应的依赖包

```
sudo apt-get install libtool
sudo apt-get install automake
sudo apt-get install autopoint
sudo apt-get install libncurses5-dev
```

3）编译安装

```
./autogen.sh 
./configure & make
```

4）测试安装成功

```
./nsenter -V
```

5) 将nsenter加入系统环境可执行路径中

```
sudo cp ./nsenter /usr/bin
```

如何使用nsenter，非常简单，
1) 首先找到container对应的进程ID

```
sudo docker inspect --format "{{ .State.Pid }}"  
```

2) 执行nsenter获得一个shell ，假设1）获得id是4308

```
sudo nsenter  --target 4308 --mount --uts --ipc --net --pid
```

这样就进入到了container中。
好了，有的人可能会说attach不是也可以吗？为什么用这个nsenter？
个人理解两个功能是完全不一样的，attach相当于找到原始的会话shell，而原始的shell可能是一个循环程序或者web server直接在shell下运行的，这样你如果执行Ctril-C，直接导致对应的container stop，所以这不是我们期望发生的。
nsenter则是新创建的一个shell，这样就是新的会话。

参考资料：

1. Why there is no nsenter in util-linux?
  http://askubuntu.com/questions/439056/why-there-is-no-nsenter-in-util-linux
2. 在 UOS 上体验 CoreOS
  https://www.ustack.com/blog/running-coreos-on-uos/
3. Attaching to a container with Docker 0.9 and libcontainer 
  http://jpetazzo.github.io/2014/03/23/lxc-attach-nsinit-nsenter-docker-0-9/
4. Docker shell/provisioning without SSH
  https://github.com/mitchellh/vagrant/issues/4179