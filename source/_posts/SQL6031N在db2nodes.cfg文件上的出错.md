title: 'SQL6031N 在 db2nodes.cfg 文件的行号"1" 上出错'
date: 2014-04-19 16:03:21
tags: DB2/Informix
---

问题是来源，迁移云环境后，最初发现keystone的log里，都是SQL链接的错误，很明显是keystone和DB2的通信出现了一点问题，
于是切换到DB2下，发现db2无法正常执行一些简单的命令比如stop，报错

```
SQL6031N 在 db2nodes.cfg 文件的行号 "1" 上出错....
```
显然错误的原因很明显，那就研究一下这个db2nodes.cfg

查了一下 DB2的官方文档，

`http://pic.dhe.ibm.com/infocenter/db2luw/v9r7/index.jsp?topic=%2Fcom.ibm.db2.luw.qb.server.doc%2Fdoc%2Fr0006351.html`

这里面涉及到了一个hostname配置，也就是说你要是主机名修改了，这个地方原来的配置hostname就会有实效的问题，
这篇blog http://blog.csdn.net/kimmking/article/details/1613430也提到了类似的问题

修改成主机新的hostname后，就没有问题了，如果改成localhost，似乎不行，/etc/hosts 有解析, 不太清楚localhost的问题，改天再研究一下。