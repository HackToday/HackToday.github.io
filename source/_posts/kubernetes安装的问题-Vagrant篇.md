title: kubernetes安装的问题-Vagrant篇
date: 2015-02-03 22:43:49
tags: 云计算
---


						如果参照安装文档，
https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/vagrant.md

实体物理机安装应该不会有问题，但是奈何资源有限，拿起来我的VirutalBox建了一个虚拟机，Ubuntu 14.04

执行安装脚本，发现了诸多问题


问题1：

==> default: Waiting for machine to boot. This may take a few minutes...
    default: SSH address: 127.0.0.1:2222
    default: SSH username: vagrant
    default: SSH auth method: private key
    default: Error: Connection timeout. Retrying...

参见 http://stackoverflow.com/questions/22575261/vagrant-stuck-connection-timeout-retrying 解决方法


问题2：

问题1会引入另外一个问题，就是图形界面的virtualbox会提示你系统不支持硬件加速，无法创建虚拟机。这是因为virtualbox不支持nested virtualization


结合问题1，2，觉得使用single node 安装方法试试。

篇外音，升级virtualbox到4.3.20后，fix了4.3.10开启3D加速系统闪退的问题。


                                   