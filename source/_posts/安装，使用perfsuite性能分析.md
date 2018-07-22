title: '安装，使用perfsuite性能分析'
date: 2011-05-17 23:57:22
tags: LINUX
---

编译过程：
首先编译之前确保安装了perfsuite组件正常工作必须依靠的软件或补丁：
参见INSTALL说明：实验环境ubuntu10.04

我的安装环境linux-2.6.36，已经包含了对performance counter的支持
安装PAPI，这个安装很简单，参见其INSTALL
expat
tcl/tk
tdom
bfd
MPI(可选)
Cube(可选)

说明：expat tcl/tk tdom 安装都比较简单（至少在ubuntu下）
直接 sudo apt-get install **

一般tcl, expat tdom都要安装dev版本. 因为没有安转dev版本，会在configure输出中有warning，
这个你只需要通过查看configure输出，确保没有warning（当然必须没有error)，configure就成功了.

/*
这里注明一下：还是有一个warning的，关于Test.class。
WARNING: I have to compile Test.class from scratch
这个没什么影响，说明编译是通的过的。
关于fortran编译器我指定的是gfortran，貌似用f77有问题
*/

还有bfd这个库，就是安装binutils-dev

1. configure配置选项，主要是因为对于默认的安装目录不一致时，你需要明确的指出。
包括papi，tdom库， linux内核的源码目录(你当前使用的内核，uname -r可以查看)
还有你要安装perfsuite的目录。
我的如下：

```
# ./configure --prefix=/home/jsi/software/perfsuite-1.0.0b1/ --with-papi=/home/jsi/software/papi-4.1.2.1/ --with-tdom=/usr/lib/tcltk/tdom0.8.3/ --with-kernel-srcdir=/usr/src/linux-2.6.36.2-change/  >output  2>&1
```

这里的output是configure过程的输出。为了方便查看。

2. 编译：n为你的cpu核数，为了加快编译速度

```
#make -jn
```

3. 运行测试suite

```
#make -s check
```

看看输出是不是都ok，如果有错误参见INSTALL中的Running the test suite一部分，我的没有

4. 安装

```
#make install
```

Finally, you can remove files created during the build to further conserve on
disk space by running:

make clean (or) make distclean

5. 体验以下perfsuite的功能

set up environment， 主要是你的安装目录不是默认的情况下，

```
#. $PERFSUITEDIR/bin/psenv.sh  
```

其中的PERFSUITEDIR是你configure中指定的安装目录
具体参见psrun等的document

6. 卸载
在你build的目录执行

```
#make uninstall
```