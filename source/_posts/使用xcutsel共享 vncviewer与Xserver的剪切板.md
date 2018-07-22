title: 使用xcutsel共享 vncviewer与Xserver的剪切板
date: 2010-07-20 11:18:18
tags: LINUX
---


						最近使用vnc很
是频繁，我的工作环境是控制端为archlinux被控端为winxp，它们之间剪切板的共享问题一直没有解决，以前少的字符总是自己打上去的，大的段落
就用ftp把文件传过去后复制粘贴，由于今天要复制的内容十分的多，终于促使我下决心解决这个问题了，在网上搜了半天终于发现原来解决方法是这么的简单，
都怪以前太懒，浪费了大把大把的时间呐。
解决问题的关键就是这个xcutsel程序，这个程序相当的简单，只有三个丑陋的按钮，分别是：
1，quit
2，copy primary to 0
3，copy 0 to primary
解释下使用法方，在windows中复制文本内容后，点击3:copy 0 to 
primary，然后在linux下点击鼠标中键或者shift+insert，即可实现clipboard transfer。
反过来，在linux下中复制文本内容后，点击2:copy primary to 0，然后就可以复制到windows中了。So Easy~~ 注：上面的内容是引用，我的系统是ubuntu 10.04其实关于从windows xp向ubuntu直接可以粘贴，不需要xcutsel的帮忙。只是从linux到xp要借助xcutsel的帮助，有个小问题就是xp到linux有时会出现内容里的引号丢失的现象，不太明白。以后再说吧，先研究程序吧。
		
		
		                                   