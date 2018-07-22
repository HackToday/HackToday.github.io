title: kubernetes安装-local cluster篇
date: 2015-02-05 18:00:30
tags: 云计算
---


	
	
		
	
	
		本文的环境搭建针对的是local cluster 方式， 
	
	
		参见 
	
	
		
	
	
		
	
	
		
	
	
		1. 除了上面的一般说明外，有几点需要注意的是，
	
	
		ubuntu安装的docker.io默认是需要root权限使用的，为了系统安装的默认用户使用方面，下面的步骤将帮助你不再每次sudo执行docker命令，[1]
	
	
		
	
	
		
	
	
		执行下面的命令：
	
	
		sudo gpasswd -a ${USER} docker
	
	
		sudo service docker.io restart
	
	
		
	
	
		然后：
	
	
		logout and relogin
	
	
		
	
	
		验证，
	
	
		docker ps 可以正常执行
	
	
		
	
	
		2. 安装etcd
	
	
		
	
	
		安装etcd比较简单，就是从
	
	
		
	
	
		下载包，然后解压即可
	
	
		
	
	
		确保对应的etcd在当前用户的$PATH设置里
	
	
		
	
	
		3. 安装kubenetes local cluster
	
	
		
	
	
		这个需要注意一点，如果第1步，没有让用户docker group中，就需要使用sudo命令，但是因为sudo命令的环境变量是使用secure_path会覆盖你写在.bashrc中的设置，所以会出现etcd无法找到的问题，解决方法最好采用1，或者修改secyre_path，其他等等...
	
	
		
	
	
		4. 还有一个问题，
	
	
		因为kubernetes的etcd检查服务用到了curl命令，ubuntu默认是不安装curl，需要安装curl，避免出现etcd错误的打印服务超时的状态。
	
	
		
	
	
		
	
	
		
	
	
		
	
	
		
	
	
		
	
	
		参考文献：
	
	
		1. 
	
	
		
	
	
		2. 
	


		
	
		本文的环境搭建针对的是local cluster 方式， 
	
		参见 
	
		
	
		
	
		
	
		1. 除了上面的一般说明外，有几点需要注意的是，
	
		ubuntu安装的docker.io默认是需要root权限使用的，为了系统安装的默认用户使用方面，下面的步骤将帮助你不再每次sudo执行docker命令，[1]
	
		
	
		
	
		执行下面的命令：
	
		sudo gpasswd -a ${USER} docker
	
		sudo service docker.io restart
	
		
	
		然后：
	
		logout and relogin
	
		
	
		验证，
	
		docker ps 可以正常执行
	
		
	
		2. 安装etcd
	
		
	
		安装etcd比较简单，就是从
	
		
	
		下载包，然后解压即可
	
		
	
		确保对应的etcd在当前用户的$PATH设置里
	
		
	
		3. 安装kubenetes local cluster
	
		
	
		这个需要注意一点，如果第1步，没有让用户docker group中，就需要使用sudo命令，但是因为sudo命令的环境变量是使用secure_path会覆盖你写在.bashrc中的设置，所以会出现etcd无法找到的问题，解决方法最好采用1，或者修改secyre_path，其他等等...
	
		
	
		4. 还有一个问题，
	
		因为kubernetes的etcd检查服务用到了curl命令，ubuntu默认是不安装curl，需要安装curl，避免出现etcd错误的打印服务超时的状态。
	
		
	
		
	
		
	
		
	
		
	
		
	
		参考文献：
	
		1. 
	
		
	
		2. 
	