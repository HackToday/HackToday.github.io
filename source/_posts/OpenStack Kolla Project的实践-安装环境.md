title: 'OpenStack Kolla Project的实践-安装环境'
date: 2015-08-31 17:36:52
tags: 云计算
---

OpenStack Kolla项目是一个支持Openstack的服务以容器的方式部署，借助ansible部署工具可以简单的扩展到多个节点
体验Kolla项目的第一步是搭建一个简单的开发环境，环境搭建的all-in-one参考官方的github 如下

https://github.com/stackforge/kolla/blob/master/docs/dev-quickstart.rst

其中比较trick的地方需要注意不同操作系统对于kernel的需求，支持的版本等。
我们以ubuntu 14.04为例，因为kernel的版本编译问题，aufs是不被3.13 kernel支持的，如果使用aufs，需要确保kernel升到3.19以上。还有一种方法是让docker使用btrfs。

我们这里谈谈方案1， 升级kernel：

1. 升级kernel

```
 apt-get install linux-image-generic-lts-vivid
```

2. 下载Kolla代码，pip安装

```
git clone http://github.com/stackforge/kolla
cd kolla
sudo pip install -r requirements.txt
```

3. 安装Docker

```
curl -sSL https://get.docker.io | bash
```

4. 安装Openstack client需要的一些包

```
sudo apt-get install -y python-dev python-pip libffi-dev libssl-dev
```

5. 安装OpenStack client

```
sudo pip install -U python-openstackclient
```

6. 禁止本机的libvirt启动， 如果以前没有安装，可以跳过这一步

```
service libvirtd disable
service libvirtd stop
```

7. 安装Ansible （或者使用pip方式）

```
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```

8. 本地Build Image， 因为远程的pull image 速度太慢 而且 Kolla 社区不是每个commit修改都把image build一遍，
所以本地build image是开发最好的选择。我们使用source方式build， binary方式似乎不稳定，容易出错

```
tools/build.py --base ubuntu --type source --template -T 35
```

9.  部署容器

```
ansible-playbook -i inventory/all-in-one -e @/etc/kolla/globals.yml -e @/etc/kolla/passwords.yml site.yml
```

使用docker ps 可以查看对应openstack 所有服务的容器，使用命令行一样简单的部署虚拟机，只不过我们的openstack服务都运行在容器中了，呵呵。