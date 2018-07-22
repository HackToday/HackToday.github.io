title: 'VirtualBox - 自动调整屏幕大小，显示分辨率'
date: 2011-10-22 09:45:48
tags: LINUX
---

在VirtualBox中安装了Ubuntu后，Ubuntu的屏幕调整不太好，操作起来非常不方便，需要安装Vbox的增强功能。具体如下：

1. 在  设备--》 安装增强功能这时会自动加载VBOXADDITIONS的虚拟光盘

2. /media/VBOXADDITIONS_4.0.10_72479 （4.0.10_72479是版本号）找到对应的操作系统的文件，比如Linux的是，VBoxLinuxAdditions.run

3. 运行这个文件，需要管理员权限

4. 操作完成，重启Ubuntu

5. 然后，就会，发现其中的菜单项“Switch to Seamless Mode”可以使用了

6. 点击“自动调整窗口大小”菜单项，这样屏幕分辨率会随着屏幕大小自动调整，非常方便

参考资料：http://aofengblog.blog.163.com/blog/static/631702120101117104437593/                                   