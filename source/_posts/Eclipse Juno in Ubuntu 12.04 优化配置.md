title: Eclipse Juno in Ubuntu 12.04 优化配置
date: 2013-11-10 10:36:16
tags: LINUX
---


						如果你在用Ubuntu 12.04和eclipse 开发环境，希望这篇文章包含的信息对你有帮助

前提：
本文是在virtualbox的虚拟机里运行的Ubuntu 12.04

问题：
1. eclipse Juno 4.2的性能问题网上讨论的可谓多如牛毛，有兴趣的可以自行google
2. 默认的eclipse 配置 + 默认的Ubuntu 配置会有些问题
  a) eclipse 有点慢， 切换tab， 代码跳转等。 （eclipse配置）
  b) 当鼠标停在某处，显示javadoc 或者pydoc的开不清楚。 （Ubuntu 配置）

解决：
资料[1]给出了详细的解决方法，为了方便参看，特摘要如下：
  问题1： 修改 eclipse.ini配置，增加如下几项
   -server  
   -Xmn128m  
   -Xms1024m  
   -Xmx1024m 

 问题2： 三种方法（资料1给出了两种方法，下面的方法2,3）
  方法1： 修改Ubuntu的theme， 见资料[2].，主要是安装gnome-tweak-tool，修改默认的配置theme： Ambiance为其他
  方法2： 不修改theme， 修改颜色配置，
       安装 gnome-color-chooser， change colors by going to Specific→Tooltips options 打开
  方法3： 不修改theme，不安装gnome-color-chooser，直接修改theme的配置文件
    /usr/share/themes/Ambiance/gtk-2.0/gtkrc
     修改如下两种配置属性 （黑色字体，黄色背景）
        tooltip_fg_color:#000000
         tooltip_bg_color:#f5f5c5
   
  
其他问题：
还有virtualbox本身的性能调整，具体不在这里展开了，资料[3]给出了一些说明

参考资料：
1. http://ubuntu-user-tricks.blogspot.com/2012/09/3-things-to-do-after-installing-eclipse.html
2. http://www.wikihow.com/Change-Themes-on-Ubuntu-with-Gnome-Tweak-Tool
3. http://blog.jdpfu.com/2012/09/14/solution-for-slow-ubuntu-in-virtualbox                                   