title: iptables 学习
date: 2012-01-03 16:44:44
tags: LINUX
---


						Linux下通过iptables设置可以开启相应的端口，有选择的开启访问权限。设置不同机器之间的互访。
    (1)查看本机关于IPTABLES的设置情况
    # iptables -L -n
    (2) 开启相应的端口（以22端口为例）
    # iptables -A INPUT -p tcp --dport 22 -j ACCEPT


Ubuntu下增加了ufw更加方便的tool来进行设置，
     (1)开启 
          sudo ufw enable
 （2）允许外部访问80端口      sudo ufw allow 80 
更多参考：1. http://www.uplinux.com/shizi/wenxian/4113.html2. http://wiki.ubuntu.org.cn/UFW%E9%98%B2%E7%81%AB%E5%A2%99%E7%AE%80%E5%8D%95%E8%AE%BE%E7%BD%AE                                   