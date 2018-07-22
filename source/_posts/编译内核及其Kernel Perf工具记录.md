title: 编译内核及其Kernel Perf工具记录
date: 2016-06-20 22:17:28
tags: LINUX
---


						最近在尝试编译安装最新的4.6.1内核，遇到一些问题，记录如下:

1. 编译内核的错误： Alert: /dev/disk/by-uuid/**  does not exist

 这个主要原因是相应的driver没有编译进内核导致。主要是相应的顺序错误, make install 是最后一步，不能在模块编译安装前运行。

make

make modules

make modules_install

make install

2. 对应的perf tool对应的ubuntu安装包没有相应的内核的版本，需要自己编译
步骤；

cd /usr/src/linux-4.6.1
cd tools/perf

make
make install


http://askubuntu.com/questions/50145/how-to-install-perf-monitoring-tool
http://stackoverflow.com/questions/34473447/custom-linux-kernel-build-failure-in-vmware-workstation
https://lkml.org/lkml/2016/5/3/3                                   