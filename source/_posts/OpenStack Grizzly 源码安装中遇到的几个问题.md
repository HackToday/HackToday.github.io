title: 'OpenStack Grizzly 源码安装中遇到的几个问题'
date: 2013-01-31 22:16:11
tags: 云计算
---

Grizzly Update

问题1： 

When you keystone endpoint-list, hit issue,
(ENVOSTest)[root@localhost bin]# keystone endpoint-list
Failed to load keyring modules.

it missing keyring lib in python,  use pip install to fix it.

pip install keyring

Blog http://blog.dynamichosting.biz/category/openstack/ seems not work with yum ways

问题2： 问题已经解决，

由于@wuwenxiang “PKI需要Openssl加密token，估计你没有在配置目录加上ssl的密钥”

[signing]
#token_format = PKI
把PKI改成UUID

refer：https://gist.github.com/4070200
否则会出现problem fulfilling your request. Command 'openssl' returned non-zero

问题3：

openstack bug
NameError: global name 'service_ref' is not defined
https://bugs.launchpad.net/nova/+bug/1102596
