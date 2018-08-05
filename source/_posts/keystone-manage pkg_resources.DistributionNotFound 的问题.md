title: 'keystone-manage pkg_resources.DistributionNotFound 的问题'
date: 2014-02-21 16:53:16
tags: 云计算
---

因为devstack安装后，需要经常pull新的代码，由于自己的开发环境不是每次都是fresh的，所以再次运行stack.sh有时会遇到一些问题。

在运行的时候，发现keystone的token的一个问题，怀疑pki的相关问题，使用keystone-manage发现，提示

```
pkg_resources.DistributionNotFound:keystone==2014.1.***
```

于是检查keystone-manage的代码，发现这个里面的代码还有  require("**keystone****") 版本依赖，
这个内容和keystone-manage的源码完全不一样。难道是偶尔运行ubuntu的apt-get install keystone导致的？ 
根据使用习惯，我是从来不用ubuntu的apt方式来装openstack。所以，要想解决这个问题，就修改keystone-manage为原来的代码。

再次运行，就没有问题了。
