title: '搭建proxy server的尝试'
date: 2015-12-01 23:08:59
tags: LINUX
---

在ubuntu下搭建一个代理服务器比较简单，ubuntu下有个squid3用的比较广泛，简单的几个步骤可以搞定。 具体实践如下:


1. 安装

```
sudo apt-get install squid3
```

2. 配置

```
sudo cp /etc/squid3/squid.conf /etc/squid3/squid.conf.original
sudo chmod a-w /etc/squid3/squid.conf.original
```

3. sudo vim /etc/squid3/squid.conf

```
acl src 

#
# INSERT YOUR OWN RULE(S) HERE TO ALLOW ACCESS FROM YOUR CLIENTS
#
http_access allow 
```

4. sudo service squid3 restart

如果启动了ufw，需要保证对应的proxy端口打开。比如你的squid设置的

```
# Squid normally listens to port 8888
http_port 8888
```

```
sudo ufw allow 8888/tcp

sudo ufw status
```

参考资料：

https://help.ubuntu.com/lts/serverguide/squid.html
http://www.tecmint.com/install-squid-in-ubuntu/