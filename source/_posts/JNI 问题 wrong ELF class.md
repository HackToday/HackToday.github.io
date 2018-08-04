title: 'JNI 问题 wrong ELF class'
date: 2012-07-24 21:55:47
tags: Java
---

使用JNI发现一个问题，
1. wrong ELF class: ELFCLASS64)
主要是机器是64位的OS，默认编译的.so是64位

而java设置的默认是32位 JDK， 所以会出现这个问题。
那么就采用编译成32位的.so， 安装 glibc-devel.i686
然后编译指定 -m32 就可以了，

2. 如果执行出现Not found in java.library.path)，这是因为JVM没有找到相应的native library，
那么就需要设置相应的 path 可以通过 

```
java -Djava.library.path='.' HelloWorld
```

或者

```
LD_LIBRARY_PATH=`pwd`
export LD_LIBRARY_PATH
Java HelloWorld
```

这样就搞定了

JNI简单过程：

```
1）创建一个Java程序，定义原生的c/c++函数
2）javac编译
3）javah -jni声称.h文件
4）创建对应的.c文件，实现对应的.h定义的函数
5）编译.c 生成.so
6) 运行java程序
```

参考资料：

http://www.cnblogs.com/xiaoxiaoboke/archive/2012/02/13/2349775.html
http://java.sun.com/developer/onlineTraining/Programming/JDCBook/jniexamp.html#examp
http://blog.csdn.net/mdemonhunter/article/details/6254478