title: 'Linux下删除减号开头的文件'
date: 2016-11-24 08:57:41
tags: LINUX
---

个人无意使用原因产生了一个临时文件，发现类似 “-2015testdoc”

这种文件不是使用常规或者\转移符类似的方式，需要使用类似下面的方式

```
rm -- "-2015testdoc"
```

关于创建类似

```
touch -- -2015testdoc 或 touch ./-2015testdoc 可以创建
```

参考资料：

1. http://blog.csdn.net/starperfection/article/details/50310711

                                   