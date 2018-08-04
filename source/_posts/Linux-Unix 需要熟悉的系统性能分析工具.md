title: 'Linux-Unix 需要熟悉的系统性能分析工具'
date: 2012-05-01 11:15:13
tags: LINUX
---

Linux下的性能分析工具主要是sysstat工具集：

1） nicstat: 搜集和监控网卡的I/O

```
# ./nicstat.sh -i em1 5
    Time      Int   rKB/s   wKB/s   rPk/s   wPk/s    rAvs    wAvs %Util    Sat
11:36:57      em1   13.05    0.75   11.12    7.44  1202.4   103.2  0.11   0.00
11:37:02      em1    0.11    0.11    1.60    1.60   67.75   70.00  0.00   0.00
```
下载地址：http://sourceforge.net/projects/nicstat/files/
可以用于分析分布式的java应用程序的网络I/O瓶颈

2）iostat ： 分析硬盘I/O利用率

```
# iostat -xd 5
Linux 3.3.2-6.fc16.x86_64 (localhost.localdomain) 	05/01/2012 	_x86_64_	(4 CPU)
Device:         rrqm/s   wrqm/s     r/s     w/s    rkB/s    wkB/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
sda               1.99     4.42    7.66    3.12   148.28    80.20    42.39     0.33   30.76   12.18   76.45   4.90   5.28
```

3）vmstat： 分析内存的利用率

```
# vmstat
procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0  60692 172812 175668 1690636    0    1    36    20  226   25  3  1 95  1  0	
 si,so 内存的换入换出数据是初步衡量性能的依据
```

4）vmstat, top 分析CPU的利用率：

```
# vmstat
procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
1  0  64812 166964 176412 1694400    0    1    36    20  226   47  3  1 95  1  0	
```
r 是衡量当前cpu的等待执行的进程队列长度

5）pidstat: 分析潜在进程的竞争resource问题（比如lock）

```
# pidstat -w -I -p 1231 5
12:03:07 PM       PID   cswch/s nvcswch/s  Command
12:03:12 PM      1231     38.60      0.20  libvirtd
```

AIX: http://www.ibm.com/developerworks/cn/aix/library/au-aix7optimize1/index.html
Sysstat: http://sebastien.godard.pagesperso-orange.fr/


附注：
如果在64位操作系统，编译nicstat会报类似的错：

```
gnu/stubs-32.h: No such file or directory
```

方案1： 编译选项改成 -m64
方案2： 安装相关的32位库， 具体参考：

http://docs.redhat.com/docs/en-US/JBoss_Enterprise_SOA_Platform/4.3/html/Getting_Started_Guide/appe-install_jdk_rhel.html#sect-use_alternatives_to_set_default_JDK
