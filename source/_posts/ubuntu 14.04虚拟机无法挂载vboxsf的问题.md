title: 'ubuntu 14.04虚拟机无法挂载vboxsf的问题'
date: 2014-06-17 11:17:09
tags: LINUX
---

环境：virtualbox 4.3.10
虚拟机： ubuntu 14.04

问题：

今天要挂载一个share folders 突然发现无法挂载，然后查了一下，原来是bad link导致的问题，修复后，正常使用。

```
test@test-VirtualBox1404:~$ sudo mount -t vboxsf vm-mount /mnt/share
mount: wrong fs type, bad option, bad superblock on vm-mount,
       missing codepage or helper program, or other error
       (for several filesystems (e.g. nfs, cifs) you might
       need a /sbin/mount. helper program)
       In some cases useful info is found in syslog - try
       dmesg | tail  or so
```

解决方法：

```
sudo ln -s /opt/VBoxGuestAdditions-4.3.10/lib/VBoxGuestAdditions /usr/lib/VBoxGuestAdditions
```

参考帖子：

http://askubuntu.com/questions/458286/getting-an-error-wrong-fs-type-bad-option-bad-superblock-on-ubuntushared