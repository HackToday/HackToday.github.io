title: '不同console下的vim之间的copy'
date: 2014-04-13 17:26:48
tags: LINUX
---

以前都是一个vim下的tabnew或者vsp之前的操作，因为是一个vim实例，所以快捷键copy和cut，paste是没有问题
今天发现不同console下的vim如果按照老套路是不能工作的，也就是说我们需要解决不同vim之间的内容如何copy，cut，paste呢 ？

其实vim的教程里说明了这一点，需要系统剪切板的支持，但是普通的vim，一般系统默认装的可能并不支持，

```
vim --version | grep clipboard
```

如果输出有：

```
-xterm_clipboard
```

那么说明你的vim有点弱，需要增强版的vim，
那么可以安装vim-gnome或者vim-gtk
安装后，vim --version, 就会有+xterm_clipboard

那么就尽情的享用快捷命令吧：

"+2yy – copy two lines to X11 clipboard
"+dd – cut line to X11 clipboard
"+p – paste X11 clipboard

具体更多参考：

http://vim.wikia.com/wiki/Accessing_the_system_clipboard                                   