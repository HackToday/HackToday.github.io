title: '解决Notes的"The remote server is not a known tcp-IP host"问题的方法'
date: 2012-03-24 09:17:03
tags: LINUX
---

转载：http://www.cnblogs.com/tohen/archive/2006/10/26/540216.html

方法一：在场所中的连接中，把该服务器连接删除，重新建一个。
方法二：文件—>设置场所—>编辑当前场所—>连接配置向导，重新配置服务器连接一遍。
方法三：在host文件中加上该服务器IP与服务器名字的解释。(host文件提供的是个静态解析,对与win系统，通常存放在c:\window\system32\drivers\etc里面)

注意：重新登录一次Notes!

我使用了方法2，很简单，ok.