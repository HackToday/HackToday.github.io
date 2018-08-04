title: 'VM cannot start because the saved state Error'
date: 2013-10-22 22:04:18
tags: 虚拟化
---

今天遇到这个问题，发现有些人找到了解决办法，如下

解决办法:

Oracle VM VirtualBox管理器主界面（GUI）
控制->清除保存的状态(I)

http://blog.sina.com.cn/s/blog_4c451e0e0101635c.html

其实这个状态一般可能是主机的不正常关机或者虚拟在保存过程中的强行退出容易出现的。多谢上面的一位兄弟指出的解决方法