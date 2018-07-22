title: Linuxkit 中的 kernel+initrd 镜像启动过程
date: 2017-05-31 08:16:04
tags: 云计算
---


	Linuxkit 中有一种镜像输出方式是 kernel+initird，以这类镜像为例，我们就来谈谈一个可运行的操作系统启动过程“”：

	

	1. 开机，主板上电 CPU Reset，寄存器初始化，根据 CS、IP 寄存器（FFFF0h） 运行第一条指令地址，由于处于内存高位地址，所以一般是一个简单的跳转指令（JMP），找到对应的BIOS 程序

	

	2. 启动 BIOS 进行 POST 自检 （内存等硬件自检），然后 BIOS 会查找可引导的设备，找到后也就是对应的引导扇区 MBR，加载内存中，然后执行

	

	引导扇区比较小，只有 512 字节，所以肯定无法完成后续的操作系统加载初始化的所有工作，所以这里就包含了一些关键信息：如何继续启动过程和装载 OS 的一些信息，也就是包含分区表，Boot Code（查看哪个分区是 Active）Boot code 执行后就会把控制权交给对应 Active 分区的 boot 部分

	

	3. 接下来就是 Bootloader（比如 GRUB） 部分，装载 kernel 和 initrd

	

	initrd 就是 Ram Disk，可以被 kernel 解压使用，用来作为临时的 root 文件系统，直到真正的 root 文件系统被挂载。

	initrd 主要包含了可以挂载真正 root 文件系统的程序和一 些必要的驱动

	

	kernel 和 initrd 配合完成上面的，就开始将控制权交给 /sbin/init (对于 initramfs 则是 /init)，/sbin/init 主要会加载所有对应的 service 和用户空间的工具（配置的内容在 /etc/inittab 文件中），挂载对应 /etc/fstab 下的分区

	

	4. 用户进入登录界面

	

	

	特别说明，关于 initrd 和 initramfs 区别，在于

	initrd 和 initramfs 都是作为根文件系统被挂载前的临时文件系统，被内核调用

	

	区别是：(更详细参考文献 12)

	initrd 是 linux kernel 2.6 前引入的， 是一个 gzip 压缩的文件系统类型的镜像，需要内核中相应的模块编译才能完成对应的加载（比如loop 设备） 

	initramfs 是 linux kernel 2.6 引入的，一个 gzip 压缩的 cpio 归档，是一个内存盘，不依赖于底层的文件系统

	

	

	

	具体查看对应镜像中的内容如下：

	

	

	# mkdir /tmp/initrd
# cp initrd.img /tmp/initrd
# gunzip < /tmp/initrd/initrd.img
# mkdir /mnt/initrd
# mount –ro loop /tmp/initrd/initrd /mnt/initrd
# cd /mnt/initrd
	
		
	


		
	
	

	# gunzip -c /boot/initrd-2.6.31.img > /tmp/my_initrd

	# cpio -it < /tmp/my_initrd         [examine contents]

	 lib

	 lib/udev

	 lib/udev/console_init

	 lib/firmware

	 lib/firmware/radeon

	 lib/firmware/radeon/RV730_me.bin

	 ... snip ...

	 lib/modules

	 lib/modules/2.6.31

	 lib/modules/2.6.31/radeon.ko

	 lib/modules/2.6.31/modules.isapnpmap

	 ... and so on and so on ...

	# cd [somewhere]                    [in preparation for unloading]

	# cpio -i < /tmp/my_initrd          [unload]

	

	

	

	参考：

	

	[1]http://flint.cs.yale.edu/feng/cos/resources/BIOS/

	[2]https://www.bbsmax.com/A/pRdBnwg9dn/

	[3]http://www.dewassoc.com/kbase/hard_drives/master_boot_record.htm

	[4]https://www.pks.mpg.de/~mueller/docs/suse10.1/suselinux-manual_en/manual/cha.boot.html

	[5]https://www.novell.com/documentation/suse91/suselinux-adminguide/html/ch12s03.html

	[6]https://www.kernel.org/doc/html/v4.10/admin-guide/initrd.html

	[7]

	[8]

	[9]

	[10]

	[11]

	[12]

	[13]https://www.zhihu.com/question/22364502

	[14] 

	[15] 

	[16] [17] https://ksearch.wordpress.com/2010/09/30/extract-contents-of-initrd/
