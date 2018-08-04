title: 'Linux误删系统文件修复'
date: 2012-08-25 17:11:50
tags: LINUX
---

因不注意删除了 /var/lib 下的文件，所以需要找个方法解决,

虽然，在删除资料后，不推荐在对磁盘的写操作，但是机器的硬盘的因为特殊原因不能自行拆卸，也没有实现安装修复软件。所以只能先搞安装修复软件了。

首先，原来的/var/lib下有yum相关的文件，导致无法yum安装，
解决如下：

```
http://www.ogre.com/taxonomy/term/33

yum failures with missing $releasever
Posted June 15th, 2011 by leif

    Fedora
    Linux

During an upgrade (yum update) on a Fedora VM, something went horribly 
wrong, and it crashed in the middle of the update. After rebooting, and 
cleaning up the mess, yum still was very unhappy. Running an update 
would give me errors like

Could not parse metalink https://mirrors.fedoraproject.org/metalink?repo=fedora-$releasever&arch=x86_64 error was 
No repomd file
Error: Cannot retrieve repository metadata (repomd.xml) for repository: fedora. Please verify its path and try again

Very odd. It turns out, $releasever was not properly set, and I could 
not figure out why. Poking around, I realized that $reelasever is 
supposed to come from examining the version number of a particular RPM 
package, in my case fedora-release. Well, lo and behold, this package 
was no longer installed on my box, yum must have uninstalled it, but 
crashed before installing the new update (or something...). I mounted 
the Fedora Core DVD, and simple reinstalled the missing package, and 
things are happy joy joy again. Here's the command:

$ sudo rpm -i ./Packages/fedora-release-13-1.noarch.rpm
```

其次，找到  http://hi.baidu.com/cnjxxf/item/7b92070f59a09390a3df4392
extundelete  修复ext4 ext3
那么就可以安装这个软件，然后修复了。 修复后的会放在 RECOVERED_FILES一个目录里，
然后我copy到/var/lib，毕竟对磁盘做了一些写操作（安装上述的软件及依赖），所以有些文件估计恢复不了。


然后，我检查了恢复差不多，然后在重启机器前，把所有的数据备份好，以防止机器起不来。


好在机器能起来，但是无法进入桌面系统。 好像gdm出问题了，重装gdm也不行，最后想到kdm。
安装kdm后，重启后就可以进入桌面了。

                                   