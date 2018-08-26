title: '编译 QEMU 报错 configure.ac:14: error: possibly undefined macro: AC_PROG_LIBTOOL'
date: 2017-05-25 13:36:04
tags: 虚拟化
---

在 CentOS 7.2 上编译最新的 QEMU 2.9.0 版本的时候，发现除了常规的

```
yum install gcc gcc-c++  zlib-devel glibc-devel automake autoconf
```

依赖包，还会报另外一个错误

```
configure.ac:14: error: possibly undefined macro: AC_PROG_LIBTOOL
      If this token and others are legitimate, please use m4_pattern_allow.
      See the Autoconf documentation.
```

这个错误可以安装如下的包来解决：

```
yum install libtool
```