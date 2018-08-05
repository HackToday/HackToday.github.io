title: 'Docker的操作学习-attach,detach,volume data'
date: 2014-10-01 15:15:48
tags: 虚拟化
---

Docker作为另一类虚拟化技术-容器方案，已经吸引了越来越多的开发和关注，docker在各大操作系统厂商-Ubuntu，Redhat等都提供了很好的集成，更给力的是docker的官方的文档写的非常清晰易懂。下文以ubuntu 14.04为例，对docker的CLI进行简单的学习，


Docker Hub上提供了大量image可以进行测试练习，所以我们随便选一个

1. 启动进入一个shell

```
sudo docker run  -i -t  --name web training/webapp /bin/bash
```

2. Detach 上面的container

``` 
CTRL-p CTRL-q
```

3. Attach上面的container

```
sudo docker attach web
```

4. 以background方式运行 container

```
sudo docker run  -d -P  training/webapp python app.py
```

其中 -P 是将container的ports暴露给对于主机所有的interface，比如上面的启动的container，
我们可以通过sudo docker ps 查看运行的端口，

```
f3ca5cd8c307        training/webapp:latest     /bin/bash     24 minutes ago      Up 22 minutes   0.0.0.0:49154->5000/tcp   web  
```

这样，如果Host有多块网卡，每个网卡有不同的ip，所有的ip:49154 都可以访问这个web app了。      

5. Docker的数据管理及使用       

 docker的Data volumes功能可以:

 - 对container进行方便的volume创建，
 - 对host的目录进行快捷的mount到container
 - 不同container之间通过volume进行数据共享
 - 对data volume进行方便的backup，restore和migrate

例如：

- 创建一个名字是dbdata的container包含dbdata的volume

```
sudo docker run -i -t -v /dbdata --name dbdata training/postgres /bin/bash
```

- 对上面的container的/dbdata挂载到新的container db1上

```
sudo docker run -i -t  --volumes-from dbdata --name db1 training/postgres /bin/bash
```

backup 和 restore:

1. 首先启动一个新的container来对dbdata的data volume数据打包备份到host的当前目录下

```
sudo docker run --volumes-from dbdata -v $(pwd):/backup training/postgres tar cvf /backup/backup.tar /dbdata
```

2. 创建一个用来restore上面数据的container，名字是dbdata2

```
sudo docker run -v /dbdata --name dbdata2  training/postgres /bin/bash
```

3. un-tar到新创建的container的data volume中

```
sudo docker run --volumes-from dbdata2 -v $(pwd):/backup training/postgres tar xvf /backup/backup.tar
```

很明显，上面的几个步骤通过Host来实现了文件的转存。

作为外延，读者可以参考更多的资料：

1. Docker的技术预览
   http://www.infoq.com/cn/articles/docker-core-technology-preview
2. Docker的源码分析
   http://www.infoq.com/cn/articles/docker-source-code-analysis-part1?utm_campaign=infoq_content&utm_source=infoq&utm_medium=feed&utm_term=global&utm_reader=feedly
3. Docker的CLI
   http://docs.docker.com/reference/commandline/cli/
   http://docs.docker.com/userguide/dockervolumes/
   http://docs.docker.com/userguide/usingdocker/