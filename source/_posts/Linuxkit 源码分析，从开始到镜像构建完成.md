title: Linuxkit 源码分析，从开始到镜像构建完成
date: 2017-06-08 22:32:39
tags: 云计算
---


	在上一篇【1】中我们介绍了 “Linuxkit 中的 kernel+initrd 镜像启动过程”，那么我们接下来看看 Linuxkit 是如何一步一步构造出 kernel +initrd 这些镜像的。

首先看一下 Moby  结构体定义，基本和 yml 中支持的属性一一对应

	

	23 // Moby is the type of a Moby config file

	 24 type Moby struct {         

	 25     Kernel struct {

	 26         Image   string     

	 27         Cmdline string     

	 28     }

	 29     Init     []string     

	 30     Onboot   []MobyImage   

	 31     Services []MobyImage   

	 32     Trust    TrustConfig   

	 33     Files    []struct {   

	 34         Path      string   

	 35         Directory bool     

	 36         Symlink   string   

	 37         Contents  string   

	 38     }

	 39     Outputs []struct {     

	 40         Format string     

	 41     }

	 42 }

	 43

	

	NewConfig（parse yml config 文件）   --> buildInternal (实际的 build 过程)  -->  outputs （生成相应格式的镜像）

	

	其中 buildInternal 的过程如下

	

	   --> dockerPull   --> ImageExtract  --> untarKernel

	 

	下载 Kernel 对应的 image，从对应的镜像中提取文件系统并返回一个 tar，从 tar 中提取出 kernel 、 kernel.tar 并放到新的 tar 文件中（initrdAppend 操作），tar 文件为了区分 kernel 和 其他文件，对 kernel 设置了 tar header 字段（boot/kernel boot/cmdline）

	

	对于 Init 镜像都要解压，并且添加到上面新的tar 文件中（initrdAppend）

	

	对于 Onboot 镜像，执行以下操作：

	ConfigToOCI(image) ，   --> ImageBundle（对应目录为/containers/onboot/）  --> 同样添加到文件中 （initrdAppend）

	

	对于 service 镜像，执行以下操作：

	 ConfigToOCI(image)    --》 ImageBundle（对应目录为 /containers/services/）  --> 同样添加到文件中 （initrdAppend )

	 

	最后是添加一些额外的文件，程序需要的，比如 ssh key 文件（initrdAppend）

	

	-->  outputs 

	按照 yml 中的输出格式，输出对应的文件，这里以 kernel+initrd 举例

	输出对应的 kernel + initrd 文件，其中 kernel 和 initrd 是在前面的 tar 包装的时候，都有设置了对应的 tar header，所以可以从一个文件中抽取出 kernel，剩下的就是 initrd 文件

	

	对于其他虚拟机镜像格式，其实就是讲对应的 kernel + initrd 打包成对应的镜像格式比如 qcow2

	

	

	那么对于上述构造的 VM 镜像，里面的容器如何自动运行起来的。那么我们就需要理解前面讲的 kernel +initrd 的启动过程，在 BIOS 自检后，bootloder 开始加载 kernel +initramfs， 那么我们从下面将这里如何对应到 moby 构建的镜像里面的代码

	

	initrd 上面除了包含一些 modules， 还包含 init， onboot， services 镜像，其中 kernel+ initrd 配合完成对应的真正 root 挂载后，控制权交给了/sbin/init

	

	

	init 进程，开始根据  /etc/inittab 中进行进程的派生，看卡 inittab 什么样子

	

	/ # cat /etc/inittab

	# /etc/inittab

	

	::sysinit:/etc/init.d/rcS

	::once:/etc/init.d/containerd

	::once:/etc/init.d/containers

	

	# Stuff to do for the 3-finger salute

	::ctrlaltdel:/sbin/reboot

	

	# Stuff to do before rebooting

	::shutdown:/usr/sbin/killall5 -15

	::shutdown:/bin/sleep 5

	::shutdown:/usr/sbin/killall5 -9

	::shutdown:/bin/echo "Unmounting filesystems"

	::shutdown:/bin/umount -a -r

	ttyS0::once:cat /etc/issue

	ttyS0::respawn:/sbin/getty -n -l /bin/sh -L 115200 ttyS0 vt100

	tty0::once:cat /etc/issue

	tty0::respawn:/sbin/getty -n -l /bin/sh  38400 tty0

	

	

	

	我们看到了熟悉的 /etc/init.d/containerd 和  /etc/init.d/containers，其中 containerd 是容器运行控制进程，首先启动。接着对于所有的containers 进行 roofs 的挂载，runc 调用运行这些容器，代码如下：

	

	/ # cat /etc/init.d/containers

	#!/bin/sh

	

	# start onboot containers, run to completion

	

	if [ -d /containers/onboot ]

	then

	    for f in $(find /containers/onboot -mindepth 1 -maxdepth 1 | sort)

	    do

	        base="$(basename $f)"

	        /bin/mount --bind "$f/rootfs" "$f/rootfs"

	        mount -o remount,rw "$f/rootfs"

	        /usr/bin/runc run --bundle "$f" "$(basename $f)"

	        printf " - $base\n"

	    done

	fi

	

	# start service containers

	

	if [ -d /containers/services ]

	then

	    for f in $(find /containers/services -mindepth 1 -maxdepth 1 | sort)

	    do

	        base="$(basename $f)"

	        /bin/mount --bind "$f/rootfs" "$f/rootfs"

	        mount -o remount,rw "$f/rootfs"

	        log="/var/log/$base.log"

	        ctr run --runtime-config "$f/config.json" --rootfs "$f/rootfs" --id "$(basename $f)" $log >$log &

	        printf " - $base\n"

	    done

	fi

	

	wait

	

	通过mount 查看里面的挂载情况：

	

	tmpfs on /var type tmpfs (rw,nosuid,nodev,noexec,relatime)

	tmpfs on /containers/onboot/000-sysctl/rootfs type tmpfs (rw,relatime)

	tmpfs on /containers/onboot/001-sysfs/rootfs type tmpfs (rw,relatime)

	tmpfs on /containers/onboot/002-binfmt/rootfs type tmpfs (rw,relatime)

	tmpfs on /containers/onboot/003-format/rootfs type tmpfs (rw,relatime)

	tmpfs on /containers/onboot/004-mount/rootfs type tmpfs (rw,relatime)

	tmpfs on /containers/services/dhcpcd/rootfs type tmpfs (rw,relatime)

	tmpfs on /containers/services/docker/rootfs type tmpfs (rw,relatime)

	tmpfs on /containers/services/ntpd/rootfs type tmpfs (rw,relatime)

	tmpfs on /containers/services/rngd/rootfs type tmpfs (rw,relatime)

	

	如此，就完成了 VM 启动后的容器的自动运行

	

	

	参考：

	1. http://blog.chinaunix.net/uid-21335514-id-5765657.html
2. https://en.wikipedia.org/wiki/Tar_(computing)

	

	

	

	
