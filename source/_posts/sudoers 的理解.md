title: 'sudoers 的理解'
date: 2012-03-24 09:18:59
tags: LINUX
---

转载： http://apps.hi.baidu.com/share/detail/33864699

用sudo时提示"xxx is not in the sudoers file. This incident will be reported.其中XXX是你的用户名，
也就是你的用户名没有权限使用sudo,我们只要修改一下/etc/sudoers文件就行了。

下面是修改方法

1）进入超级用户模式。也就是输入"su -",系统会让你输入超级用户密码，输入密码后就进入了超级用户模式。（当然，你也可以直接用root用） 
2）添加文件的写权限。也就是输入命令"chmod u+w /etc/sudoers"。 
3）编辑/etc/sudoers文件。也就是输入命令"vim /etc/sudoers",输入"i"进入编辑模式，找到这一 行："root 
ALL=(ALL) ALL"在起下面添加"xxx ALL=(ALL) ALL"(这里的xxx是你的用户名)，然后保存（就是先按一 
下Esc键，然后输入":wq"）退出。 
4）撤销文件的写权限。也就是输入命令"chmod u-w /etc/sudoers"。原文档介绍的其他的很多，不再转载，这里仅仅留作记录查看。                                   