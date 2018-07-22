title: linux下恢复ntfs分区的误删文件
date: 2011-05-26 10:11:43
tags: LINUX
---


						linux下恢复ntfs分区的误删文件今天操作文件的时候，稍不留神，直接shift+del一个重要文件，很郁闷。但是想想windows下自己试过不少恢复软件，能够工作的不错，linux下应该也很多，所以就决心寻找丢失的文件。查了个debugfs，实在用不来，恢复老出问题。后来用了ntfsundelete终于搞定！所以写下来，也算给大家些资料。恢复过程：1. 确定你删除文件的分区，这个用fdisk -l就可以了。（比如 /dev/sda6)2. 卸载误删的分区，因为会检查ntfs卷是否处于打开状态。也不用卸载，直接使用-f选项，不进行检查。3.查看最近三天（这个在参数里可以设置，就是下面的3d）删除的文件$ sudo ntfsundelete  /dev/sda6  -f -t 3d这时会列出，下面的信息：Inode  Flags   %age  Date      Size        Filename2012    .........   .....    ...       603136       2022    .........   .....    ...          70 ....Files with potentially recoverable content 8还好，表明还有8个文件可以恢复。因为列出来的文件名都是none，那我怎么知道我要恢复的文件？ 笨方法是一个个恢复，不过大家都不笨，根据时间，大小的信息可以推断。注意列出来的时间可不是删除文件的时间。就像你ls列出文件的时间，这个是你最近修改文件的时间戳。我删除的是word文档，比较大，一眼能看出来，而且是昨天修改保存的。所以直接锁定2012这个inode。4. 恢复文件到指定的目录（如 /home/nice)$ sudo ntfsundelete /dev/sda6 -f -u -i 2012  -d  /home/nice5. /home/nice下就出现一个unknown的文件，查看是不是你的文件。哦，god，谢天谢地，真是原来的word文档，ntfsundelete这家伙不赖！参考资料：http://www.06net.com/article/20110412/75860.html                                   