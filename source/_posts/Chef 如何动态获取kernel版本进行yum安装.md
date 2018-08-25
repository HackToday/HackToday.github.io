title: 'Chef 如何动态获取kernel版本进行yum安装'
date: 2015-07-29 22:25:48
tags: Python/Ruby
---

我们使用redhat的时候，有时候对配置节点需要安装指定版本的kernel-devel，但是这个版本是动态获取的，不是hardcode的，
这就出现一个简单的问题，如何编码？

Chef有Package resource可以做这个事情，package需要至少两个属性，package 的名字和版本，如果不指定版本，就按照yum repo安装最新的。可是我们对版本有要求，所以必须指定version。

其实kernel的版本多和node的属性有关系， [2]给出了一个解决方法，但是发现在我的chef-server下，那样编码不对。 需要这样做：

```
kernel_release = node['kernel']['release']
kernel_version = kernel_release.sub(".#{node['kernel']['machine']}", '')


package 'kernel-devel' do
  version kernel_version
  action :install
end
```

参考文献:

[1] https://docs.chef.io/resource_package.html
[2] http://lists.opscode.com/sympa/arc/chef/2015-03/msg00295.html                                   