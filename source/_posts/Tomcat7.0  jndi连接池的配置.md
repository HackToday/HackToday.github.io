title: 'Tomcat7.0  jndi连接池的配置'
date: 2011-09-04 15:30:41
tags: Mysql/postgreSQL
---

测试环境：

```
ubuntu 11.04
Kernel: 2.6.38
Mysql: 5.1.54
Tomcat: 7.0
eclipse: 3.6
Mysql数据库驱动： mysql-connector-java-5.1.17.zip
```

因为打算看看后端的数据库操作，所以就搭了环境试验一下，对于普通的数据库连接（使用JDBC的方式，比较简单，所以就试验jndi 连接池）

1. 搭建好tomcat服务器

在mysql建立一个BookDB的数据库，建立一个books表，内容如下：

```
mysql> select * from books;
+-----+-----------+------------------------+-------+------+--------------------------------+------------+
| id  | name      | title                  | price | yr   | decription                     | saleAmount |
+-----+-----------+------------------------+-------+------+--------------------------------+------------+
| 201 | wangfang  | Java programming guide | 33.75 | 1999 | Guide user to grasp quickly    |       1001 |
| 202 | zhangbing | Weblogic tech          | 50.25 | 2000 | Good to learn web logic        |       2000 |
| 203 | sunyan    | Oracle Database        |    60 | 2002 | database entry book            |       2700 |
| 204 | Aling     | Hadoop Guide           |   100 | 2004 | A cookbook for distrbuted Arch |       5700 |
+-----+-----------+------------------------+-------+------+---------------------
```

2. 将Mysql驱动中得jar文件拷贝到  {TomcatHome}/lib/ 

3. 使用eclipse建立一个Dynamic Web Project, 在META-INF/ 下建立context .xml

```
xml version="1.0" encoding="UTF-8"?>

<Context>

<Resource auth="Container"

driverClassName="com.mysql.jdbc.Driver"

maxActive="100"

maxIdle="40"

maxWait="12000"

name="jdbc/BookDB"

username="root"

password="test"

type="javax.sql.DataSource" url="jdbc:mysql://localhost:3306/BookDB?characterEncoding=UTF-8" />

Context>
```

4. 在WEB-INF/ 下创建  web.xml
 
```
'-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN'
'http://java.sun.com/j2ee/dtds/web-app_2_3.dtd'>
 
      DB Connection
      jdbc/BookDB
      javax.sql.DataSource
      Container
``` 

5. 创建jsp文件， index.jsp

```
<%@ page language="java" import="java.util.*,javax.naming.*,java.sql.*,javax.sql.*"pageEncoding="UTF-8"%>

<%      

    Context ctx = new InitialContext();        

    String strLookup = "java:comp/env/jdbc/BookDB";   

    DataSource ds =(DataSource) ctx.lookup(strLookup);  

    Connection con = ds.getConnection();  

    if (con != null){  

        out.print("success");  

   }else{  

        out.print("failure");  

    }         

%>
```

6. 运行，OK