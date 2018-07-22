title: python 相同的 url 下载行为不一致调查 （Tornado）
date: 2017-08-24 08:23:58
tags: Web开发
---


	

	

	
		
	
	
		
	

		
	
		
	
	mimetypes.py 中会根据不同的文件扩展类型来推测 mime_type ，一台机器是 application/octet-strea，另外一台是 text/plain

	这就奇怪了，同样的操作系统和 tornadoweb 版本

	

	再细看 mimetypes.py 源码发现，里面有一些蹊跷

	

	

	其中 /etc/mime.types 一台机器上没有，另外一台包含， 这个文件中包含了 .log 结尾的文件情况，那么看看是哪个包导致的不同：

	

	# rpm -q --whatprovides /etc/mime.types

	mailcap-xxx-.el7.noarch

	

	后来确认才知道，一台机器当时使用的最小化安装，所以遗漏了一些后期的标准化安装的包。这个差别才导致了行为的差异。
