title: Devstack安装碰到python package的版本问题
date: 2013-09-03 11:53:45
tags: 云计算
---


						因为一直在用devstack在一台虚拟机器上进行安装，openstack的havana的开发过程相关的依赖版本会不时的更新，最近安装devstack发现总会出现一些service启动不了
直接查看log一般是 version 不匹配，或者缺少相关的module，显然是相关的package安装不匹配的缘故，如何解决


sudo pip install --upgrade  -r /opt/stack/nova/requirements.txt

如果安装还是失败，报version不匹配，那就在 /usr/local/lib/python2.7/dist-packages/ 删除相应的package，（如果你的pip uninstall可以work的话， 那就最好用pip uninstall）
再运行上面的命令。                                   