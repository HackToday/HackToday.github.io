title: 'OpenStack的Instance私钥访问'
date: 2015-02-04 18:11:16
tags: 云计算
---

如果使用openstack的key-pair add后，产生的私钥是pem文件，这个是无法直接通过putty访问的。

需要经过类似的一个key转化的步骤，具体参加这篇blog
http://blog.sina.com.cn/s/blog_575b2c50010198oy.html

然后就可以使用putty访问了， 本文针对的是windows下的用户，如果不用cygwin的方式。                                   