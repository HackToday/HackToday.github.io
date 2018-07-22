title: 强大的cscope+vim+ctags
date: 2010-12-16 23:41:02
tags: LINUX
---

强大的cscope+vim+ctags今天终于打算弄弄linux下的cscope这套工具了，以前总是没用到地方 。看到了http://blog.csdn.net/jsufcz/archive/2009/03/13/3988883.aspx 写的一篇文章，进行了相关的验证，发现阅读代码很是方便。实验环境：Ubuntu-9.10 1. 首先确认vim支持cscope，方法： vim --version | grep cscope 如果显示 +cscope 就是支持了；再安装cscope。如果不支持，似乎要重新编译vim和下载cscope（这个没有验证，ubuntu默认的支持）。2. 其次，安装ctags3. 然后就是具体操作了，体验强大之处吧，比如你一个目录下存放的一个软件的源代码，那么进入目录中，进行如下的操作：    操作1， ctags -R    这一步是为了递归生成标签文件操作2， cscope-indexer -r   这一步是生成索引信息文件操作3， cscope 使用cscope查看源代码了。 还是很方便的。附上一点常用命令：（转自：http://blog.csdn.net/jsufcz/archive/2009/03/13/3988883.aspx）在vim中ctags的简单使用
1) 跳转到指定的函数进入vim后，用 “:tag func_name“ 跳到函数func_name处。使用tag
命令时，可以使用TAB键进行匹配查找，继续按TAB键向下切换。
某个函数有多个定义时

:tag
跳到第一个定义处，优先跳转到当前文件
:tnext
跳到第一个
:tfirst
跳到前count个
:[count]tprevious
跳到后count个
:[count]tnext
跳到最后一个
:tlast
你也可以在所有tagname中选择：
:tselect tagname

如果想跳到包含block的标识符:“tag /block” 然后用TAB键来选择。这里'/'就是告诉vim
'block'是一个语句块标签。
2)用“CTRL + ]“快捷键，跳转到光标所在函数标识符的定义处。
3)使用“CTRL + T”退回上层。如果想在以write_开头的标识符中选择一下， :tselect /^
write_ 这里，'^'表示开头，同理，'$'表示末尾。多个同名的标识符
感谢前人开发的工具和网友在配置上做的探索。