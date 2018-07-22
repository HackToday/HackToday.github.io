title: 追踪无法umount的来源
date: 2014-04-19 15:54:28
tags: 云计算
---


						前段时间执行一个cookbook，发现节点执行完后，每次总是有个路径没有正常umount，于是我就陷入深深的郁闷中，可是工程师的职责就是解决问题的，别人不帮你解决，那就得自己搞定。

首先，要让完整的异常信息给抛出来，到底是什么原因导致的无法umount
如果你查看过 chef 的代码说明https://github.com/opscode/chef/blob/master/lib/chef/mixin/shell_out.rb
shell_out 可是对错误不报警的
shell_out! 绝对不容忍命令执行错误，绝对抛出异常

使用shell_out!， cookbook执行后，把错误面目暴露出来了， device is busy
很显然是某个进程使用这个设备

然后，我们就需要找到哪个进程在用这个device， fuser 或者lsof 就是干这事的，做调查的
比如fuser
sh -c fuser -m  /tmp/mytest ; ps aux

将上面的命令输出结果在cookbook执行时，打印出来，就可以抓到进程了


最后，上一步既然获得了进程也知道是chef-client在用，那就是cookbook中存在使用目录的情况，后面的就简单了，一般就是打开文件，没有关闭，或者进入到了要卸载的目录
grep 找到open file的地方，发现有一个地方没有close，就是它了。

Fix问题后，重新运行cookbook，发现所有umoun就顺利完成了。问题就解决了。                                   