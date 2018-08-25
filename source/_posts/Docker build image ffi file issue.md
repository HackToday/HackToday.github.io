title: 'Docker build image issue for fatal error: ffi.h: No such file or directory'
date: 2015-05-18 11:29:38
tags: Python/Ruby
---

如果使用base的Ubuntu 14.04的image，会发现在在安装python的cryptography package,会报错，

```
 fatal error: ffi.h: No such file or directory
  #include 
                  ^
 compilation terminated....distutils
````

根据， http://www.123code.blogspot.com/2014/10/fixed-installing-python-cryptography.html,
需要安装两个 `libssl-dev` `libffi-dev`包

                                   