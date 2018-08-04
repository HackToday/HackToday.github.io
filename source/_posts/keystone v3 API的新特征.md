title: 'keystone v3 API的新特征'
date: 2013-02-24 20:16:23
tags: 云计算
---

keystone的v3 API与v2.0相比有很大的不同，从API的请求格式到response的返回结果都有差别，主要几点如下：

1. 引入了domain的概念，domain是在project，user， group之上抽象出的一个概念，是指

container for projects, users and groups

2. v3中用project代替了以前的v2.0的tenant概念

3. v3的验证/auth/tokens,相比v2.0的/tokens，token的ID不再在body中包含，而是在返回header中的X-Subject-Token

4. v3还在验证上引入plugin方式，

```
[auth]
methods = password,token
password = keystone.auth.methods.password.Password
token = keystone.auth.methods.token.Token
```

可以添加自己的plugin来对keystone自己的auth方式进行定制化，关于auth这一部分：因为token有scoped和non-scoped的区别：

```
Scoped
1. project 
If a project is specified by name, then the domain of the project must also be specified in order to uniquely identify the project
2.  domain
Alternatively, a domain name may be used to uniquely identify the project.

A token scoped to a project will also have a service catalog, along with the user's roles applicable to the project. Example response:
A token scoped to a domain will also have a service catalog along with the user's roles applicable to the domain. Example response:


Non-scoped
1. user
If the user is specified by name, then the domain of the user must also be specified in order to uniquely identify the user

```

因为社区的开发方式，keystoneclient的开发和keystone并不是完全同步，加上keystone的v3还没有完全开发完毕，
所以现在用的keystoneclient还不完全支持v3 keystone。如果你要提前用keystoneclient，那就需要修改其中v3的client的代码

5.兼容性

为了让v2.0 的scheme迁移到v3，v2.0会默认的使用(keystone,conf)

```
# default_domain_id = default
```

更多的参考资料，可以参看：

1. v3 API design https://etherpad.openstack.org/grizzly-keystone-v3api

2. v3 API doc https://github.com/openstack/identity-api/blob/master/openstack-identity-api/src/markdown/identity-api-v3.md


	
