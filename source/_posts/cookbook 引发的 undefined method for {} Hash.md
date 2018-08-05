title: 'cookbook 引发的 undefined method for {} Hash'
date: 2014-04-01 17:52:25
tags: Python/Ruby
---

在写cookbook的时候，发现strainer test 一直有问题
undefined method `<<' for {}:Hash

比如network的cookbook在havana中spec_helper.rb是

```
# README(galstrom21): This will remove any coverage warnings from dependent cookbooks
ChefSpec::Coverage.filters << '*/openstack-network'
```

出错的地方，是filters这个地方

调查发现，原来社区的cookbook在icehouse将chefspec的version升级到3.4了
而havana是使用的3.1.4

查看chefspec的ruby class 说明：
chefspec 3.1.4 资料[1]

```
# File 'lib/chefspec/coverage.rb', line 16
def initialize
  @collection = {}
  @filters = []
end
```

这个是array

chefspec 3.4 资料[2]

```
# File 'lib/chefspec/coverage.rb', line 28
def initialize
  @collection = {}
  @filters = {}
end
```

已经变为hash了，所以显然如果仅仅更新GemFile，还是基于havana的spec_helper直接运行，那么就会报上面的错误，所以社区已经修改使用了如下的调用

```
ChefSpec::Coverage.start! { add_filter 'openstack-network' }
```

更多参考可以看社区的 blueprint

1. http://rubydoc.info/gems/chefspec/3.1.4/frames
2. http://rubydoc.info/gems/chefspec/ChefSpec/Coverage#filters-instance_method
3. https://review.openstack.org/#/c/83712/
4. https://launchpad.net/openstack-chef
