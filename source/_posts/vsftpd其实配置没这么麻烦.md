title: vsftpd其实配置没这么麻烦
date: 2011-03-04 00:02:31
tags: LINUX
---


						今天打算在ubuntu server上装个ftp服务器软件，发现了vsftpd.然后按照http://wiki.ubuntu.com.cn/Vsftpd%E5%AE%9E%E4%BE%8B进行配置，发现使用设置的用户给/var/ftp/pub上传不行，提示：200 PORT command successful. Consider using PASV.
不能create file也就是说没写的权限。   网友说是权限没设置对，不过已经弄成775了，搞不成要777，貌似有的测试不行，说归结到selinux相关的问题，好多给的方案是：1. setsebool -P ftpd_disable_trans 1
2. service vsftpd restart
发现这个根本不起作用。嗨，发现这个其实说的很简单，    http://forum.ubuntu.com.cn/viewtopic.php?f=54&t=115655&start=0你设置成建立用户对应于home下的目录就行了，整/var下既麻烦，还要禁止selinux，实在不怎么好。    对了，如果你设置disable selinux，那怎么改回去，这个有点意思。终于找到一个给力的帖子：http://www.crypt.gen.nz/selinux/disable_selinux.html运行 fixfiles relabel 再 reboot。搞了半天，没有大繁化简，。。。。。。                                   