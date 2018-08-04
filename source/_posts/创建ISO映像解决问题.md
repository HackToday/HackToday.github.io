title: '创建ISO映像解决问题'
date: 2013-07-20 12:13:37
tags: LINUX
---

1. 问题来源：

   我找不到原始的iso镜像了，只在一台机器上发现拷贝的文件，所以我需要利用这些文件制作成iso来安装。

2. 最初的参考资料：

如果参考 http://linuxlookup.com/howto/create_iso_image_file_linux中的mkisofs方法
在制作ISO的时候会遇到问题，无法正常的boot

```
Steps to follow
Create an ISO image from optical media
In this example, we're going to copy the contents of a disk in the CD/DVD drive (/dev/cdrom) to an ISO image file. Open a terminal window and type the following at the command line.

dd if=/dev/cdrom of=/directory/example.iso

Notations:
- dd is the program used to convert and copy a file.
- if defines an input file.
- of defines an output file.
- iso is the resulting ISO image file.
Create an ISO image from files in a directory
To create an ISO image from files within a directory is just as simple. State an output directory and name of the ISO to create, along with a source directory. For example:

mkisofs -o /home/linuxlookup/example.iso /source/directory/
```

3. 问题:

关于从directory制作iso会遇到几个问题，
- "mkisofs: Uh oh, I cant find the boot catalog directory "

需要确保，isolinux/isolinux.bin 和 isolinux/boot.cat 相对应的iso目录正确
参考：
http://www.linuxquestions.org/questions/linux-software-2/mkisofs-returns-error-mkisofs-uh-oh-i-cant-find-the-boot-catalog-directory-844905/

- boot image './isolinux/isolinux.bin' has not an allowable size.

这个是因为默认的是floppy，解决方法
你需要加入: -hard-disk-boot 或者 -no-emul-boot.
参考：
http://www.linuxquestions.org/questions/red-hat-31/mkisofs-error-boot-image-%27-isolinux-isolinux-bin%27-has-not-an-allowable-size-358439/ 

4. 解决方法:

我的例子（redhat x86_64 6.4 ISO)：

```
sudo mkisofs  -o ~/images/rhels64.iso  -b isolinux/isolinux.bin  -c  isolinux/boot.cat -no-emul-boot -boot-info-table -l -r -J -v -allow-lowercase ~/images/rhels6.4/x86_64/
```

说明：关于link中给出的  -boot-load-size 4，这个我的没有指出也可以，是因为，

```
-boot-load-size load_sectors   Specifies the number of "virtual" (512-byte) sectors to load in no-emulation mode. 
The default is to load the entire boot file. Some BIOSes may have problems if this is not a multiple of 4. 
```

