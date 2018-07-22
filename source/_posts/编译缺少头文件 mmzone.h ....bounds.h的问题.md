title: 编译缺少头文件 mmzone.h ....bounds.h的问题
date: 2010-12-24 08:23:33
tags: LINUX
---


						
	出现这个问题，大都是在内核源码目录外编译产生的，google了一下，发现一篇文章，原来这些问题是在kbuild时产生的，内核本身没有，这够晕的。。。


	见下面解决方法：


	来源：http://blog.csdn.net/wby0322/archive/2010/05/26/5624565.aspx


	出现的问题：编译的时候提示缺少头文件 mmzone.h ....bounds.h...等


	include/linux/mmzone.h:18:26: error: linux/bounds.h: No such file or directory
include/linux/mmzone.h:197:5: warning: "MAX_NR_ZONES" is not defined


	原因：bounds.h是在编译内核时生成的，类似于编译产生的.o文件，如果你运行

"make clean" or "make distclean"，这个文件就会被清除掉（详情查看内核Makefile）。因此，如果再利用此内核源码编译内核模块，
如果有涉及bounds.h，就会出现找不到该文件的错误。

	解决：独立内核目录之外编译模块时，要确保makefile文件中所定义的内核源代码树已经make过一遍，且没有make  clean。这样就不会清除生成的bound.h头文件，这个文件是生成模块必须的。


	或者"make prepare"

这样就会重新生成bounds.h，搞定了！                                   