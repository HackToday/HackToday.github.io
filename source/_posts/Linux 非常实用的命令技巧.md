title: 'Linux 非常实用的命令技巧'
date: 2017-03-06 08:43:01
tags: LINUX
---

1.格式化表格输出命令结果
通过 column 命令来格式化结果

例如：  mount | column -t

因为默认的以空格作为分隔符，如果对于一些命令的输出分隔符不是空格的情况下，使用下面的方式：
还有类似的 cat /etc/passwd  | column -t -s :

2. 重复的执行命令直到成功
这个没有什么特别的的地方，使用 while 循环

3. 巧妙的使用sort来对某些列进行排序

例如根据内存使用来对进程进行排序：
ps aux | sort -nk 4

4. 同时查看监控多个log文件

单个文件的查看的时候通常是使用 tail，如果同时查看多个文件的话使用multitail 命令【TODO】

5. 回退到上一次目录

cd -

6.
Make a Non-Interactive Shell Session Interactive
To do this, change the settings from ~/.bashrc to ~/.bash_profile.

个人注：【这个目前在平常没有发现使用的场景 ，不做进一步的介绍】

7. 固定间隔下监控命令的输出

```
watch df -h
```

8.在session（shell）结束的时候仍然保持程序的运行

```
nohup wget site.com/file.zip
```

这个也可以通过screen 来实现

9. 在一些交互式的命令中自动的回答yes 和 no
通过yes 命令来完成，
例如：

```
yes | apt-get update
```

如果是想回答no的话，使用 yes no | command

10.创建一个特定大小的文件，
使用dd 命令来完成，
例如:

```
dd if=/dev/zero of=out.txt bs=1M count=10
```
11.以root身份运行上一个命令
sudo !!

12. 记录你的Linux 终端回话记录
这个不是单纯的history 记录执行的命令，它除了记录执行的命令，还有执行的结果。
使用方式：
script

....  执行的命令

exit

它会自动生成一个typescript 文件，包含了你说执行的命令和输出结果

同时可以结合 scriptreplay 来回放你刚才执行的过程。


13. 使用tab 来替换掉空格
通过使用tr命令，
例如:

```
cat geeks.txt | tr ':[space]:' '\t' > out.txt
```

14. 使用xargs 命令来完成一些高级的管道命令操作

```
find. -name *.png -type f -print | xargs tar -cvzf images.tar.gz
```

注意这里默认是使用上一个命令的输出给xargs命令的最后面

如果你的命令需要的是中间的位置，比如 cp xx  /somepath

这样可以借助 {} 和 -i 参数

例如：

```
ls /etc/*.conf | xargs -i cp {} /home/likegeeks/Desktop/out
```

参考：

https://dzone.com/articles/most-useful-linux-command-line-tricks