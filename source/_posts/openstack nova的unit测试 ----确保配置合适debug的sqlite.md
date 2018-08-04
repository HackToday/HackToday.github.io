title: 'openstack nova的unit测试 ----确保配置合适debug的sqlite'
date: 2012-12-02 00:00:00
tags: Python/Ruby
---

1. 问题

在你进行unittest的时候，尤其涉及到database相关的调试，总发现nova的没有生成database，其实主要是nova默认的会使用sqlite的内存内型数据库

参考sqlite的资料发现
e = create_engine('sqlite://')

The sqlite :memory: identifier is the default if no filepath is present. Specify sqlite:// and nothing else: 

2. 解决

为了调试数据库更加简单，你可以配置不使用内存型的数据库

在 nova 的TestCase (nova/nova/test.py)

fake_flags.set_defaults(FLAGS)   --->

配置类似如下的sql连接，nova.sqlite是对应的sqlite数据库文件

```
    conf.set_default('sql_connection', "sqlite:////var/lib/nova/nova.sqlite")
```

配置前面的前提是你自己的环境已经有这个数据库文件，如果没有的话，你需要手动创建一个
步骤如下：

(1) change your sql connection in nova.conf to be sqlite
(2) nova-manage db sync 
这样你对应的nova.conf设置的位置生成相应的数据库文件
(3) cp nova.sqlite clean.sqlite

完毕，这样进行unit测试最后的结果就会写到sqlite数据库文件里的。


3. 附加资料

如何查看sqlite数据库？

很多工具可以查看， 我用了两个都还可以，一个是sqliteman，前面的一篇文章已经写过了。
另外一个是SQLite Manager，这个就是一个firefox的插件，
`https://addons.mozilla.org/en-us/firefox/addon/sqlite-manager/`
很容易就可以安装为firefox的插件。
