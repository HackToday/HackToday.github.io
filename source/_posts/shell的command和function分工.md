title: 'shell的command和function分工'
date: 2013-09-08 10:37:58
tags: LINUX
---

如果你经常编写shell脚本，有时会碰到类似的问题：
shell中定义了一个函数，这个函数和系统内建的命令同名， 例如

```
function cp() {
    echo "This is a test"
    cp 
}
```

显然上面的是一个递归函数，这个递归没有退出条件，最后必然是导致Segmentation fault

如果我们不希望cp()里的继续调用自己，而是系统拷贝的命令，怎么办？
答案就是使用command命令，代码如下

```
function cp() {
    echo "This is a test"
     command cp  ...
}
```

这样就保证了系统命令覆盖函数的优先使用权。