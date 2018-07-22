title: static binary 安装docker遇到Devices cgroup isnt mounted
date: 2016-03-25 17:31:46
tags: 云计算
---


						实验环境： ubuntu 14.04
docker 版本： 1.10.3
kernel： 3.13.**

如果采用binary docker的使用方式
https://github.com/docker/docker/blob/master/docs/installation/binaries.md

你会发现docker启动，报错Devices cgroup isn't mounted

这个是因为默认的cgroup的cpu，memory等没有被挂载，需要使用一个脚本来挂载，具体使用脚本是
https://github.com/tianon/cgroupfs-mount/blob/master/cgroupfs-mount

执行完上面的脚本后，然后就可以运行docker就不会报cgroup的错误了。

这个问题解决了，但是会遇到另外一个
AppArmor enabled on system but the docker-default profile could not be loaded， 这个正在跟踪docker官方的issue，后面调查结果会随后更新
                                   