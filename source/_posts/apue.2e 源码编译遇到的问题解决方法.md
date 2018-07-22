title: 'apue.2e 源码编译遇到的问题解决方法'
date: 2010-07-20 11:12:16
tags: LINUX
---


						apue.2e 源码编译遇到的问题
1. 修改Make.defines.linux的WKDIR 
2. 然后make
error:
提示threadctl中的getenv1.c的ARG_MAX没有定义
添加#define ARG_MAX 4096 
error:
getenv3.c出现同样的问题
同样如上添加
最后成功


gcc 4.4.3
OS：Ubuntu 10.04

也是从网上的一些资料学习的，在此感谢那些人们。                                   