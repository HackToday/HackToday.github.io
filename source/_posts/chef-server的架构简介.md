title: chef-server的架构简介
date: 2014-07-15 16:38:01
tags: 云计算
---


	1. 总体架构

	chef-server，控制节点，主要存储相关的cookbook,配置节点的策略，描述注册节点的元数据

	安装节点，使用chef-client来向server节点获取配置信息，包括recipe，templates，file等，完成节点的配置工作。

	

	

	

	

	chef为了更好的支持并发，分布式环境，在11.X版本前段的server采用了erlang开发，也就是上面的Erchef 部分。

	

	

	各个组件的部分，

	

	(1)Nginx，前端的http请求的负载均衡

	(2) WebUI，提供更友好的GUI方式使用chef

	(3)Erchef，Chef-server的API实现层

	(4) Bookshelf， 是一种对象存储。按照文件的checksum存储，所以同样的文件只会存取一次，这样不同版本的cookbook的重复的文件不会存储多次。具体可以查看 /var/opt/chef-server/bookshelf/data/bookshelf/目录下的文件

	(5) Message Queues，采用的rabbitmq，

	     chef-expander 从消息队列里取出消息，处理成对应的格式，提供给chef-solr做索引

	     chef-solr对apache solr包装，提供可以进行索引和查找的REST API

	     索引数据存储的位置是  /var/opt/chef-server/chef-solr/data 

	(6) PostgreSQL,  chef-server的数据存储，

	    查看具体数据库表结构：

	      export PATH=$PATH:/opt/chef-server/embedded/bin/

	      psql -U opscode_chef

	      \l 列出所有数据库

	      \dt 列出当前连接数据库的表

	  数据库相关的存储数据， /var/opt/chef-server/postgresql/data

	

	

	2. 核心API server的组件架构

	Erchef具体架构细节：

	

	根据上面的结构图，可以看到对象的更新时chef_objects和bookshelf的完成

	chef_authn：实现了chef相关的http请求的签名和验证协议

	chef-index：和apache solr的交互，对象的增删改，和rabbimq交互。 

	  例如：

	      %% @doc Delete an object from Solr.

	chef_objects：定义了chef内部的一些对象，environment，node，data bag等

	chef_db: chef-server的数据库层逻辑，

	chef_wm： 包含了erchef的rest api实现和路由定义

	

	

	3. Chef-client执行流程：

	 A “chef-client run” is the term used to describe a series of steps that are taken by the chef-client when it is configuring a node. The following diagram shows the various stages that occur during the chef-client run, and then the list below the diagram describes in greater detail each of those stages. 

	

	

	

	

	

	1. chef-server.rb Optional Settings

	  

	

	2. Monitor

	  

	

	3. chef-client 

	 

	

	4. Erchef

	 

	

	5. chef-server

	  

	
