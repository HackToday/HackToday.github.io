title: 'screen的使用记录'
date: 2013-12-11 19:02:48
tags: LINUX
---

如果你用过devstack的话，会发现执行完stack.sh后，实际上启动一个screen会话，实现了多个进程共享一个物理终端的窗口管理器。
这个screen会话里面包含了多个screen窗口，如下

```
$ n-api  6$ q-svc  7$ q-agt  8$ q-dhcp  9$ q-l3  10$ q-meta  11$ q-metering  12$ n-cpu  13$ n-cond  14$ n-crt  15-$ n-sch  16$ n-novnc   17$ n-xvnc*  18$ n-cauth  19$ n-obj  20$ c-api  21$ c-sch  22$ c-vol
```

这样的好处很明显，

1.最大程度上实现一个物理终端的管理，简单直观。
2. 可以实现不同人之间的协作共享
3. ....


那么看到上面的说明，我们直观的想法是创建一个类似devstack的多screen窗口会话，那么怎么创建呢，具体如下：

(1) 首先要有screen会话的配置文件，目的是为了显示screen窗口的名字，这样比较直观。
简单的配置如下，创建一个文件名为.screenrc

```
hardstatus alwayslastline '%{= .} %-Lw%{= .}%> %n%f %t*%{= .}%+Lw%< %-=%{g}(%{d}%H/%l%{g})'
```

(2) 启动一个screen 会话

```
screen -dmS test1 -c .screenrc
```

(3) attach到对应的screen

```
screen -r test1
```

(4) 对应的会显示如下：

```
  0$ bash* 
```

(5) 创建一个新窗口

```
 CTRL+a  c
```

显示如下：

```
 0-$ bash   1$ bash* 
```

(6)  以此类推，可以创建2,3，...窗口

(7)对窗口重命名为自己喜欢的名字

```
CTRL+a  A
```

输入windows1 显示如下：

```
 0-$ bash   1$ windows1*  
```

(8) 关闭当前窗口

```
CTRL+a  K
```

(9) 进入拷贝/回滚模式

```
CTRL+a  ESC
```

ESC 退出拷贝/回滚模式

更多使用：

```
CTRL+a n 切换到下一个窗口
CTRL+a p 切换到前一个窗口
CTRL+a d 暂时断开screen会话
...
```

(10) CTRL +a  "  从窗口列表选择要跳转的窗口
    
```
 CTRL + a  '  会提示输入数字，切换窗口
 CTRL +a  N  （1-9） 切换到对应的 1-9 窗口
```     


更多的使用可以参考下面的参考资料：

1. http://www.cnblogs.com/taosim/articles/3270336.html
2. http://www.cnblogs.com/mchina/archive/2013/01/30/2880680.html
3. http://www.ibm.com/developerworks/cn/aix/library/au-gnu_screen/
4. http://ningg.top/linux-cmd-screen/
