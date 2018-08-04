title: 'rabbitmq epmd error  for host'
date: 2012-10-16 18:54:51
tags: LINUX
---

今天迁移虚拟机，发现死活rabbitmq起不来，提示，


```
ERROR: epmd error for host "****": timeout (timed out establishing tcp connection)
```

后来google发现，

https://gist.github.com/2522701

主机名和ip不匹配了，需要更改/etc/hosts

```
127.0.0.1 yournewhostname
```