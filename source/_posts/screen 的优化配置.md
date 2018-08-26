title: 'screen 的优化配置'
date: 2017-04-22 18:52:12
tags: LINUX
---

在原来的一篇文章中曾经提到过 screen 的一些使用方法，参见下面的内容

http://blog.chinaunix.net/blog/post/id/4033839.html

但是在使用过程中，发现一个好的 screen 配置是十分必要的，比如一个自己使用的例子如下
点击(此处)折叠或打开

```
vbell off
bell_msg ""
startup_message off
# Tell screen how to set colors. AB = background, AF=foreground
termcapinfo xterm 'Co#256:AB=\E[48;5;%dm:AF=\E[38;5;%dm'
#
# Enables use of shift-PgUp and shift-PgDn
termcapinfo xterm|xterms|xs|rxvt ti@:te@
#
# Enable 256 color term
term xterm-256color
#
# Cache 30000 lines for scroll back
defscrollback 30000
# https://wiki.archlinux.org/index.php/GNU_Screen#Start_at_window_1
bind c screen 1
bind ^c screen 1
bind 0 select 10
screen 1
hardstatus off
# https://bbs.archlinux.org/viewtopic.php?id=12571
# http://man7.org/linux/man-pages/man1/screen.1.html
hardstatus alwayslastline
hardstatus string '%{= kG}%c %06=%-Lw%{= RW}%50>%n%f* %t%{-}%+Lw%<'
```

其中最复杂的配置部分就是 hardstatus，这里 string 里的配置依次是： 设置颜色，显示标题时间（固定），当前窗口的显示颜色，窗口标号，
窗口的标志，星号，窗口标题。其中比较晦涩的地方就是缩短的控制（screen 窗口过多的情况下，避免后续新开的窗口无法显示出来，
完成一个类似可以动态窗口显示的功能）
