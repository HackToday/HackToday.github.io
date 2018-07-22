title: VNC远程登陆xfce桌面
date: 2011-04-21 01:09:19
tags: LINUX
---


						vnc登陆gnome桌面刷新屏幕速度慢的实在无法忍受，所以查查资料，发现xfce轻量级桌面会使速度加快不少，所以就试试，动手如下：1. 安装xfce桌面环境，就弄个简单的吧，      sudo apt-get install xfce4你也可以试试xubuntu-desktop2，我觉得越简单越好，所以就用xfce4就行了2. 安装x11vnc服务     sudo apt-get install x11vnc3. 打开服务准备远程登陆（******是你指定的密码，明文很恶心，不知道有没有加密的方法输入）     sudo x11vnc -forever -passwd ******4. 使用你的windows上的vnc viewer软件登陆就行了，这个简单，就是指定对应远程机器的ip，   然后根据提示的输入第3步设置的密码即可。    感谢资料：http://blog.csdn.net/kartorz/archive/2009/06/14/4268372.aspxhttp://wiki.ubuntu.org.cn/index.php?title=%E8%BF%9C%E7%A8%8B%E6%A1%8C%E9%9D%A2&variant=zh-tw                                   