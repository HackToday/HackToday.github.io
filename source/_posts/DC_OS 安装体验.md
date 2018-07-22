title: DC_OS 安装体验
date: 2016-09-12 21:42:55
tags: 云计算
---


	本文主要尝试如何快速的创建一个 DC/OS 的开发测试集群

	

	1. 安装virtualbox

	下载链接 

	根据不同的平台来安装不同的格式包，比如rpm

	下载pack：

	Oracle_VM_VirtualBox_Extension_Pack-5.0.26-108824.vbox-extpack 进行安装，点击安装

	

	2. 安装vagrant

	也是根据不同的平台来安装 

	下载链接 

	

	**注意** 1,2步骤有坑的地方是，不同的版本组合会有问题，比如我安装的时候使用的1.8.4 最新的virtualbox 5.1.4 

	vagrant是不能识别virtualbox provider的，具体错误是：

	No Usable default provider could be found for your system

	

	这个是因为有滞后支持的现象的，所以在安装的时候，根据安装需求的时候需要>=的版本跨度不要太大

	

	3. clone dcos-agent repo

	

	git clone 

	

	4. 安装 Vagrant Host Manager插件

	

	vagrant plugin install vagrant-hostmanager

	

	5. 下载DC/OS installer

	

	

	这里是1.8.2版本

	

	6. 配置DC/OS installer

	export DCOS_GENERATE_CONFIG_PATH=dcos_generate_config.sh

	export DCOS_CONFIG_PATH=etc/config-1.8.yaml

	

	7. 配置DC/OS的机器类型

	cd 
	cp VagrantConfig.yaml.example VagrantConfig.yaml

	

	8. 下载VM的基础镜像

	

	vagrant box add 

	但是这里我发现另外的机器下载很慢，于是进入链接，先下载镜像 

	下载到路径 /root/dcos-centos-virtualbox-0.7.0.box

	

	下面的格式修改如下：

	{

	  "name": "mesosphere/dcos-centos-virtualbox",

	  "description": "Base image on which to install Mesosphere DC/OS on VirtualBox",

	  "versions": [

	    {

	      "version": "0.7.0",

	      "status": "active",

	      "description_markdown": "Enable IPv4 forwarding",

	      "providers": [

	        {

	          "name": "virtualbox",

	          "url": "/root/dcos-centos-virtualbox-0.7.0.box",

	          "checksum_type": "sha1",

	          "checksum": "fc9d5a6a99ecb70aee67d4b68b8b6a48563e57cb"

	        }

	      ]

	    }

	  ]

	}

	

	执行：

	vagrant box add metadata.json

	

	9. 可选的配置： Configure 认证，（默认安装是使用OAuth，支持Google，Github, or Microsoft）

	这个会在第一次登陆的时候提示认证，当然你也可以配置不同认证，参考 

	

	10. 部署集群

	

	比如 vagrant up m1 a1 p1 boot

	

	上面的对应的是：

	m1  master节点

	a1  agent private节点

	p1  agent public 节点

	boot boot节点

	

	更多参考： 

	

	11. 扩展集群

	增加 agent node   

	vagrant up a2

	vagrant up p2

	

	减少节点

	vagrant destroy -f a1

	vagrant destroy -f p1

	

	删除整个集群

	vagrant destroy -f

	

	

	12. 简单的测试集群工作，

	

	创建一个nginx 的 job

