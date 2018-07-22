title: 安装sqliteman
date: 2012-11-28 22:17:11
tags: SQLite/嵌入式数据库
---


						sqliteman 是一款小巧的图形化管理sqlite 数据库的软件，安装相对简单因为自己使用redhat6.3，从官方给的那个源死活yum不行，就使用源代码编译安装了。 具体如下：sqliteman的编译安装依赖：cmakeqt还有stdlibc的东东注：如果没有Qscintilla2 libraries and header files， 需要使用编译选项来从源代码安装安装过程如下：安装依赖的软件1）yum install cmake.x86_642）yum install compat-libstdc++-33.x86_643）yum install qt-devel.x86_64编译4）cmake -DWANT_INTERNAL_QSCINTILLA=15） make6) make install 然后就可以使用了                                   