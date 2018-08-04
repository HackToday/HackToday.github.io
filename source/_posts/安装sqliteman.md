title: '安装sqliteman'
date: 2012-11-28 22:17:11
tags: SQLite/嵌入式数据库
---

sqliteman 是一款小巧的图形化管理sqlite 数据库的软件，安装相对简单
因为自己使用redhat6.3，从官方给的那个源死活yum不行，就使用源代码编译安装了。 具体如下：
sqliteman的编译安装依赖：

```
cmake
qt
```

还有stdlibc的东东

 **注明：** 如果没有 `Qscintilla2 libraries and header files`， 需要使用编译选项来从源代码安装

安装过程如下：

安装依赖的软件

1）yum install cmake.x86_64
2）yum install compat-libstdc++-33.x86_64
3）yum install qt-devel.x86_64

编译

4）cmake -DWANT_INTERNAL_QSCINTILLA=1

5）make

6) make install 

然后就可以使用了