title: 'ntp 同步配置'
date: 2012-04-18 23:49:55
tags: LINUX
---

以Fedora为例，

1）找到对应对ntp软件包，比如我的OS是64位，那么对应的是ntp.x86_64

```
      yum search ntp 
```

2） 安装ntp包

```
sudo yum install ntp.x86_64
```

3）开始配置ntp的 server：


sudo vim /etc/ntp.conf 

```
# Permit all access over the loopback interface.  This could
# be tightened as well, but to do so would effect some of
# the administrative functions.
restrict 127.0.0.1 
restrict -6 ::1

# Hosts on local network are less restricted.
restrict 172.16.66.0 mask 255.255.255.0 nomodify notrap
```

上面这行是允许相关的client机器可以来和server（本机）同步，

server 172.16.15.183  

配置局域网里用作NTP服务器的IP，其他的

```
server 0.fedora.pool.ntp.org iburst
server 1.fedora.pool.ntp.org iburst
server 2.fedora.pool.ntp.org iburst
server 3.fedora.pool.ntp.org iburst
```

根据的机器可以访问公网，可以有选择的注释掉。

4）配置ntp服务机器启动后自动运行，

为了使NTP服务可以在系统引导的时候自动启动，执行：

```
  chkconfig ntpd on
```

启动ntpd：

```

service ntpd start
```

5）检查对应的同步信息，
 tail -f /var/log/messages

6) 配置client端，修改 /etc/ntp.conf
加上对应的server ip即可,

```
server 172.16.15.183  
```

同样对client机器执行 4）

7） 使用 ntpstat 来检查同步状态

参考：
http://www.51testing.com/?uid-130600-action-viewspace-itemid-122930
http://www.cyberciti.biz/faq/rhel-fedora-centos-configure-ntp-client-server/