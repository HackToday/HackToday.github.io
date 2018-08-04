title: 'eclipse中openstack一些模块的unresolved'
date: 2012-09-23 11:26:57
tags: 云计算
---

openstack的git下载的源码导入到eclipse中有时会一些模块unresolved
比如paste，发现使用pip下载后的paste的package中没有__init__.py ，很奇怪

于是就从网上下载对应版本paste源码包，然后将__init__.py 导入, 执行
python setup.py install 
就会对其编译，生成__init__.pyc
重启eclispe，可以源码跳转到paste了，但是还是有个 Unresolved import: deploy

具体还没想出怎么回事，不过暂时不影响使用。