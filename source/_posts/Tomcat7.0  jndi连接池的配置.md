title: Tomcat7.0  jndi连接池的配置
date: 2011-09-04 15:30:41
tags: Mysql/postgreSQL
---


	           Kernel: 2.6.38

	           Mysql: 5.1.54

	   Tomcat: 7.0

	   eclipse: 3.6

	   Mysql数据库驱动： mysql-connector-java-5.1.17.zip

	

	因为打算看看后端的数据库操作，所以就搭了环境试验一下，对于普通的数据库连接（使用JDBC的方式，比较简单，所以就试验jndi 连接池）

	

	1.

	搭建好tomcat服务器

	在mysql建立一个BookDB的数据库，建立一个books表，内容如下：

	
		mysql> select * from books;
	
	
		
	
	
		
	
	
		
	
	
		
	
	
		
	
	
		
	
	
		
	
	
		
	

		mysql> select * from books;
	
		
	
		
	
		
	
		
	
		
	
		
	
		
	
		
	
	

	2. 将Mysql驱动中得jar文件拷贝到  {TomcatHome}

	

	

	

	

	
	

	

	

	

	

	

	

	

	

	

	

	

	

	

	

	

	
