title: pep8的问题(ubuntu12.04和Redhat 6.4)
date: 2013-08-17 08:20:37
tags: Python/Ruby
---


						前几天调试一个build失败的问题，发现服务器上的pep8检查总是无法通过，但是开发环境确没什么问题。经过调查发现，Openstack的pep8已经采用了1.4.5的版本，开发环境用的原来的1.1，新的pep8采用了更严格的格式检查，所以导致开发环境无法检查出这些问题。

ubuntu 12.04和Redhat 6.4对应的pep8源版本过旧，所以如果你用的默认的pep8，那么也会有这个问题。

而且eclipse的pydev插件用的也是旧的pep8检查。

解决方法：
升级对应的pep8，
pip install --upgrade pep8==1.4.5

如果你用的是python的虚拟环境，要检查对应的pep8版本是否一致，不一致的话，那就是requires文件里的版本配置的不对                                   