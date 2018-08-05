title: 'chef的文件传输'
date: 2014-03-23 17:40:20
tags: 系统运维
---

chef有两类resource支持文件传输，一个是remote_file, 还有一个是cookbook_file，这两个的区别是

如果要传输的文件是放在cookbooks中的file目录下的，那么需要使用cookbook_file，顾名思义，就是文件放在cookbook里
如果要传输的文件是在放在远程的一个地方，那么使用remote_file， 其实这里的远程比较广，支持

```
The location (URI) of the source file.
This value may also specify HTTP (http://), FTP (ftp://), or local (file://) source file locations
```

我们举个简单的使用场景，
比如，你要对机房管理的机器，安装一个软件包，由于内网限制或者没有软件配置源，我们提前把要安装的软件包下载到本地，
然后使用cookbook功能完成传输文件，然后安装， 简单的一个recipe 比如：

```
cookbook_file "/tmp/test.gem" do
  mode 0644
  owner 'root'
  group 'root'
  action :create
end

gem_package "test" do
  gem_binary("/opt/chef/embedded/bin/gem")
  source "/tmp/test.gem"
  action :install
end
```

如果remote_file,就是需要制定相应的uri，但是需要保证node是可以访问对应的资源的，都算可行的方法