title: PKI token 不工作的解决
date: 2013-03-13 22:52:51
tags: 云计算
---


						在keystone设置PKI的token方式下，出现一个奇怪的问题， keystone自己能工作，但是其他的service 比如 glance， nova 就是不能正常使用，
起初怀疑是conf文件的参数配置问题，后来经过多次尝试，终于发现问题的原因在于PKI的keystone-signing有错误了，我原来用过一次其他的设置 /etc/keystone/ssl/， 后来又改变了，所以都混乱了。
解决办法：

1. 找到对应的keystone的keystone-signing， 删除掉，重新启动keystone。 因为我的glance用的是这个，所以必须重启glance
2. nova的如果设置了，删除掉对应的keystone-signing-nova/

这样nova， glance的都能在PKI的token下工作了。
                                   