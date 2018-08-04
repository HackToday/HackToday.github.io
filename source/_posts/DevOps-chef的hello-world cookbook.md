title: 'DevOps-chef的hello-world cookbook'
date: 2013-09-20 17:01:12
tags: 系统运维
---

在上一篇的文章中，我们简单了介绍chef的环境搭建，那么现在你肯定就跃跃欲试如何创建一个cookbook，真正体会自动化的配置管理的便捷之处。
废话少说，我们选取简单的hello-world为例子，

需求：

1.在workstation可以让多个节点自动化创建hello-world.txt文件
2.文件的owner是test， group是test

假设：

test用户和组是在节点上已经存在


实施：
（下面的步骤如果没有特殊说明，都是在workstation上运行的）

1. 创建cookbook

```
   knife  cookbook create hello_world
```

2. 添加内容到recipe
         recipe就是主要的控制内容，让节点完成具体的操作。

cat recipes/default.rb  如下：

```
#
# Cookbook Name:: hello_world
# Recipe:: default
#
# Copyright 2013, YOUR_COMPANY_NAME
#
# All rights reserved - Do Not Redistribute
#
# recipes/default.rb
template "#{ENV['HOME']}/hello-world.txt" do
  source 'hello-world.txt.erb' 
  mode '0644'
  owner 'test'
  group 'test'
end
```

上面的指令是：在用户对应的目录（Linux/Unix：~   Windows: %HOMEPATH%）下创建一个文件hello-world.txt：
内容是放在hello-world.txt.erb模板里面。

3. 添加对应的模板

```
 cat templates/default/hello-world.txt.erb 如下：

<% # templates/default/hello-world.txt.erb %>
Hello World!


Chef Version: <%= node[:chef_packages][:chef][:version] %>
Platform: <%= node[:platform] %>
Version: <%= node[:platform_version] %>
```

4. 上传cookbook到chef server

```
   knife  cookbook  upload 'hello_world'
```

5. 将相应的cookbook添加到对应节点run_list

```
  knife node run_list add  'hello_world'
```

其中的node name，可以通过knife node list查看

6. 检查相应的run list是否被node包含

```
  $ knife node show 
Node Name:   
Environment: _default
FQDN:        
IP:          
Run List:    recipe[hello_world]
Roles:       
Recipes:     hello_world
Platform:    ubuntu 12.04
```

7. 节点应用cookbook

```
knife ssh name:  -x  -P  "sudo chef-client"
```

8. 验证node是否正确配置， 登录到node节点，检查

```
 cat ~/hello-world.txt 如下：

Hello World!


Chef Version: 11.6.0
Platform: ubuntu
Version: 12.04
```

可见按照上面的步骤我们就顺利的完成了一个很简单的cookbook开发，熟悉了流程后，后面可以继续研究复杂cookbook的编写。

其他参考：

1. http://technology.customink.com/blog/2012/05/28/provision-your-laptop-with-chef-part-1/
2. https://wiki.opscode.com/plugins/viewsource/viewpagesrc.action?pageId=18645173
