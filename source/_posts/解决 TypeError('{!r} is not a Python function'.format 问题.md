title: "解决 TypeError('{!r} is not a Python function'.format 问题"
date: 2017-01-11 16:28:56
tags: Python/Ruby
---

今天因为在一个测试环境安装更新 gnome桌面发现一个问题

```
    import requests
  File "/usr/lib/python2.7/site-packages/requests/__init__.py", line 52, in 
    from .packages.urllib3.contrib import pyopenssl
  File "/usr/lib/python2.7/site-packages/requests/packages/urllib3/contrib/pyopenssl.py", line 49, in 
    from cryptography.hazmat.backends.openssl import backend as openssl_backend
  File "/usr/lib64/python2.7/site-packages/cryptography/hazmat/backends/openssl/__init__.py", line 7, in 
    from cryptography.hazmat.backends.openssl.backend import backend
  File "/usr/lib64/python2.7/site-packages/cryptography/hazmat/backends/openssl/backend.py", line 37, in 
    from cryptography.hazmat.backends.openssl.x509 import _Certificate
  File "/usr/lib64/python2.7/site-packages/cryptography/hazmat/backends/openssl/x509.py", line 24, in 
    class _Certificate(object):
  File "/usr/lib64/python2.7/site-packages/cryptography/utils.py", line 23, in register_decorator
    verify_interface(iface, klass)
  File "/usr/lib64/python2.7/site-packages/cryptography/utils.py", line 43, in verify_interface
    actual = inspect.getargspec(getattr(klass, method))
  File "/usr/lib64/python2.7/inspect.py", line 815, in getargspec
    raise TypeError('{!r} is not a Python function'.format(func))
TypeError: is not a Python function
```

这个一般从网上找有各种各样的解答，直接照搬是没有帮助的，初步当时怀疑是python 的一些安装包版本冲突导致了，
仔细看上面的是从 requests 引发的，那么我们先通过 pip 查看对应的 requests 版本，发现有两个版本，显然是前面升级中误操作了什么导致的。

解决方法，卸载全部的requests 包，然后重新使用 pip 安装
	
