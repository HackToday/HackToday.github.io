title: Docker的操作学习-attach,detach,volume data
date: 2014-10-01 15:15:48
tags: 虚拟化
---


	
 docker的Data volumes功能可以
 （1）对container进行方便的volume创建，
 （2）对host的目录进行快捷的mount到container
 （3）不同container之间通过volume进行数据共享
 （4）对data volume进行方便的backup，restore和migrate

例如：
  (1-1) 创建一个名字是dbdata的container包含dbdata的volume
 
  
 (1-2) 对上面的container的/dbdata挂载到新的container db1上
 

	 
