title: exFAT格式的移动硬盘在Windows 7下无法写
date: 2016-07-02 15:08:06
tags: WINDOWS
---


						前段时间使用的Mac格式化的exFAT格式的移动硬盘，发现无法创建文件和删除文件，原来需要检查修复一下就可以了，

具体如下：
管理员权限运行cmd，执行下面的命令即可：

chkdsk 盘符 /F


参考:
http://blog.chenxu.me/post/detail?id=1fa055ec-b46a-4437-a92f-a5d1596ad654                                   