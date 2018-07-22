title: 'Network-manager Panel没有的问题，如何配置'
date: 2011-11-15 18:28:24
tags: LINUX
---

解决办法:编辑nm-system- settings.conf文件,将managed = false改为 true
sudo gedit /etc/NetworkManager/nm-system-settings.conf
[ifupdown]
managed=true
sudo /etc/init.d/network-manger restart

就可以看到状态为已经连接了.


注：还有一个信息和上面的无关，为了简单就记在这里，（ubuntu 10.04 更改最大化最小化关闭按钮位置 ），见参考资料2
1. Alt + F2 ，运行 gconf-editor
2. 在左侧目录树中，找到 /apps/metacity/general/
3. 在右侧找到键： button_layout ， 修改值为 menu:minimize,maximize,close

Ubuntu 关闭触摸板：
sudo modprobe -r psmouse即可。如果想打开的话可以把前面两种方式中的-r去掉即可。


来源：
[1]http://www.brucebot.com/2010/05/solve-error-of-gnome-panel-and-network-manager-under-ubuntu-1004/
[2]http://apps.hi.baidu.com/share/detail/17102222
[3] http://blog.csdn.net/iaccepted/article/details/6857454