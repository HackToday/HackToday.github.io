title: 'Linuxkit 在 OpenStack Ocota 版本的实验'
date: 2017-06-22 08:06:02
tags: 云计算
---

测试环境 CentOS 7.3

需要说明关于 Devstack 部分，不同的 OS 除了本身 Devstack 默认安装基本步骤一样，还有不同的额外步骤，有的不需要（比如下面的一些防火墙的规则，可能 Ubuntu 默认安装的就不需要）

说一下这个实验的思路，就是 Linuxkit 本身是可以 build VM 镜像的，而 OpenStack 本身又是一款包含有虚拟化管理功能的软件，那么我们希望试试 LinuxKit 自身build的镜像是不是可以很好的运行在 OpenStack 这个平台上。具体步骤如下：

1. 搭建 OpenStack 环境
方法1： 使用 Devstack
优点，简单，快速，定位解决问题方便
缺点，有些自定义的配置比较多，可以参考 Devstack 详细解释文档，一般不需要

方法2： 使用 RDO （因为是 CentOS 环境）
优点：基本不需要自定义话配置
缺点：使用 RDO 前置条件较多，除了配置源之外，还需要对防火墙等做一些处理。因为集成了 puppet， 所以不熟悉 puppet的话，出了问题，定位解决不方便。网上资料虽多，但是还是不如 DevStack 

最初尝试了 RDO，试了几次均不成功，发现除了一些模块相关的问题（我拿到手的这台 CentOS 机器也是被做了一些自定义配置，有些不知名的问题，比如 ipv4 iptables 中 filter 规则不识别，ipv6 模块加载不正常等），还最会报错 sql connection 配置没有。 

因为我们今天的重点不是研究怎么部署 OpenStack, 所以就没功夫调试 RDO 了，投入了 DevStack 的怀抱

DevStack 老方法，文档也介绍的很详细，不再累赘了，请参考文献【1】

说几个在 DevStack 中没有提到的，就是最新版的devstack 在 CentOS 安装后，需要开启对应的 80， 443 端口，要不然你在其他机器上无法访问 Horizon Dashboard， 具体就是参考文献【2】

/etc/sysconfig/iptables 加入下面的两条规则，然后重启 iptables 服务

```
-A INPUT -p tcp -m multiport --dports 443 -j ACCEPT
-A INPUT -p tcp -m multiport --dports 80 -j ACCEPT
```

2. 搭建完 DevStack 基本就完了，如果还有其他的需要的话（比如通过虚拟机ping 主机 IP），你可能在 tcpdump 发现有admin prohibited 日志，那么就需要参考文献【4】进行防火墙规则的更新。

3. 部署 DevStack 后，简单的部署一个 Cirros 镜像，然后测试 private ip 连通性，分配一个 floating ip，测试联通性（假设你添加的securitygroups 中包含了对相应协议端口的开放，比如 ICMP，DNS 等）

4. 下面开始导入 Linuxkit 镜像，这里以 sshd 镜像为例，具体怎么 Build， 在我的博客中文献【5】也做了介绍

5. 上传 sshd qcow2 镜像 （当然可以使用 kernel+inittrd ramdisk 方式） 到 OpenStack Glance， 这里简化通过 dashboard 操作，不用命令行，如下图所示：


6. 部署镜像，关联 float ip （上面的图中的ip  172.24.4.5）

7. 验证测试效果, 如下图所示：

```
[root@mytestpc ~]# ssh 172.24.4.5
Welcome to LinuxKit
moby:~# whoami
root
moby:~# ls
moby:~# ps -ef | grep ssh
  317 root       0:00 ctr run --runtime-config /containers/services/sshd/config.json --rootfs /containers/services/sshd/rootfs --id sshd
  427 root       0:00 /sbin/tini /usr/bin/ssh.sh
  524 root       0:00 /usr/sbin/sshd -D -e
  548 root       0:00 sshd: root@pts/0
  554 root       0:00 grep ssh
moby:~#
```

8. 当然我们也可以测试 linuxkit 中 redis 镜像，如下

验证如下：

```
[root@mytestpc ~]# redis-cli  -h 172.24.4.3
172.24.4.3:6379> help
redis-cli 3.2.8
To get help about Redis commands type:
      "help @" to get a list of commands in
      "help " for help on
      "help " to get a list of possible help topics
      "quit" to exit

To set redis-cli perferences:
      ":set hints" enable online hints
      ":set nohints" disable online hints
Set your preferences in ~/.redisclirc
172.24.4.3:6379>
```

参考：

1. https://docs.openstack.org/developer/devstack/
2. https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/2/html/Getting_Started_Guide/chap-Deploying_The_Dashboard.html
3. http://linuxwiki.github.io/NetTools/tcpdump.html
4. http://blog.chinaunix.net/uid-21335514-id-5403664.html
5. http://blog.chinaunix.net/uid-21335514-id-5765203.html
