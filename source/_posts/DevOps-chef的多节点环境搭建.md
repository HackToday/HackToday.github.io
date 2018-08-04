title: 'DevOps-chef的多节点环境搭建'
date: 2013-09-20 16:29:29
tags: 系统运维
---

前言：
前段时间一直想试验一下DevOps的一些配置管理工具，后来因为某些原因，就重点研究了chef。以自己的机器搭建了一个典型的多节点实验环境。

架构：
根据官方的chef的架构介绍，主要包括三大部分，

1. chef-server
2. chef workstation
3. chef-node

source:http://docs.opscode.com/chef_overview.html

上面的这个图很清晰的表达了各部分的联系，所以推测我们需要安装的主要是server和workstation部分，而node应该是通过客户端来让chef自动化安装的

实施：

下面我们介绍如何实施一个代表性的环境

环境的前提配置:

- 安装的OS环境： Ubuntu 12.04
- 虚拟化软件： VirtualBox
- 使用的网络类型： Host-Only NAT
- 假设用户都自己配置好了， 主机对应的FQDN

1. server 的安装 

http://docs.opscode.com/install_server.html 过程比较简单， 就是安装包，然后运行配置命令。
具体如下：chef_11.6.0-1.ubuntu.12.04_amd64.deb

安装：

```
    sudo dpkg -i chef-server_11.0.8-1.ubuntu.12.04_amd64.deb
```

配置:

```
    sudo chef-server-ctl reconfigure
```

验证安装：

```
    sudo chef-server-ctl test
```

其实这个似乎不能全部pass，感觉chef的集成测试集可能有些问题

2. workstation 的安装 （http://docs.opscode.com/chef/install_workstation.html）
过程稍微比server安装复杂，但还算简单明了， 主要是安装client ，配置client到server访问。具体如下：

    2.1 安装client：

	```
    sudo dpkg -i  chef_11.6.0-1.ubuntu.12.04_amd64.deb
    ```


    2.2 安装配置chef-repo:

    ```
    git clone git://github.com/opscode/chef-repo.git
    ```

	因为chef-repo是存放cookbooks的地方，knife命令行工具会从chef-repo上传数据到chef-server，
	这样chef-client就从server可以应用相应的cookbooks了，所以我们可以知道，chef-repo需要配置和server的访问,
	主要包括，.pem files and knife.rb files

    2.3 创建.chef目录

	在 chef-repo目录下，创建.chef目录，并且修改.gitignore文件，添加 .chef

    2.4 配置：

    ```
    knife configure --initial
	```

	**注意：** 输入相关的信息，主要是server的url，client的key，client注册server所需要的validator和
    validator private key（默认的是chef-validator和server端 /etc/chef/validation.pem 文件）
    还有admin的private key，所以我们需要从server端copy两个文件到workstation机器上，
    （才能在运行knife configure输入恰当的private key 信息）

	admin 和 validator的private key 文件，即： admin.pem validation.pem，

	```
    scp  root@:/etc/chef/admin.pem  ./
    scp  root@:/etc/chef/validation.pem ./
    ```

    2.5 将knife.rb和pem文件移到 chef-repo的.chef目录下 

    ```
	cp **    /.chef

	```

    2.6 验证 client 是否工作

    ```
    knife client list

    knife user list
	```

	就可以输出相关的server端的信息了


3. Node 的自动化安装 （http://docs.opscode.com/install_bootstrap.html）

   下面的是在workstation上运行的，比较简单：

    3.1 bootstrap

	```
    knife bootstrap -x -P --sudo
	```

    3.2 验证 node

    ```
    knife client list
	```

	正确的话，就会输出你的node节点的名字FQDN。
	**注意：** 因为workstation 在bootstrap的时候是需要ssh到node的，而且node也是需要到server访问的
	（依靠FQDN，就是你配置的server url),那么就意味着， node需要安装ssh server； node是可以解析server的FQDN的，
	可以在/etc/hosts添加相应的信息

总结：

经过1,2,3步骤，我们就搭建一个典型的chef 环境，包括三个节点，server， workstation和node
后面我们会给出一篇文章来说明如何创建一个cookbook，并且让node应用这个cookbook。

其他参考资料：

1. http://www.opscode.com/blog/2013/03/11/chef-11-server-up-and-running/
2. http://dev.classmethod.jp/server-side/chef-server-install/
3. http://docs.opscode.com/install.html
4. http://docs.opscode.com/chef_overview.html
5. http://jtimberman.housepub.org/blog/2013/02/10/install-chef-11-server-on-centos-6/
