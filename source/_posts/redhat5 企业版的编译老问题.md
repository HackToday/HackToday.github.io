title: redhat5 企业版的编译老问题
date: 2011-03-30 00:35:15
tags: LINUX
---

1.解压initrd文件 #mkdir /var/aa #cp /boot/initrd-2.6.30.4.img /var/aa #cd /var/aa # mkdir newinitrd # cd newinitrd/ # zcat ../initrd-2.6.30.10.img | cpio -i10236 blocks释放之后看到如下内容 lsbin dev etc init lib proc sbin sys sysroot 2.vim init#删除一下重复的两行 3.重新打包initrd #find . | cpio -c -o > ../initrd # cd .. # rm -f initrd-2.6.30.10.img  # gzip -9 < initrd > initrd-2.6.30.10.new.img # lsinitrd-2.6.30.10.new.img  initrd          newinitrd好了，initrd-2.6.30.10.new.img就是重新打包的
initrd了，然后把initrd-2.6.30.10.new.img拷贝到/boot，更改grub.conf里边的initrd-
2.6.30.10.img为initrd-2.6.30.10.new.img就可以了重新启动，好了上述出现的问题消失了！！！还有细节小问题，安装模块时，先把原来的/lib/modules/ 下对应的内核文件夹东西清空或者备份到别处，否则新编译后的安装模块会和原来的混起来，个人原出过莫名奇妙的错误，貌似和这个有点关系。反正这样做是保险点。注明：相关的解决问题出处标注了来源，对相关的网上资料感谢。