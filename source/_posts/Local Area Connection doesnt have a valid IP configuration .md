title: Local Area Connection doesnt have a valid IP configuration 
date: 2014-05-31 15:58:01
tags: WINDOWS
---


						今天办公突然发现windows7网络链接有问题了，检查网线，网线接口都没有问题，那肯定出在操作系统这边，
网络诊断提示
Local Area Connection doesn't have a valid IP configuration

莫名其妙，google一下，发现有些干货，真的很给力，解决了问题，这样你就不用苦恼的重装系统了（最笨最郁闷的方法）

帖子内容如下：（黑体字体部分就是解决方法）
Hey everyone,  I've had this problem for days with my home router on windows7.  Try these two commands and restart.  it worked for me.
Link of Website is below also.
2. Reset WinSock and TCP/IP Stack
Open a Command Prompt as administrator:
Reset WINSOCK entries: 
         netsh winsock reset catalog
Reset TCP/IP stack: 
         netsh int ip reset reset.log
Reboot the machine 
(you can run both commands first, I tend to put multiple commands in notepad and then copy and paste into the command window).
I can also confirm that this is the only solution that worked for me. I entered the second command and rebooted computer. Upon restart my internet functionality was returned.
You can find the original article on the microsoft website at: http://support.microsoft.com/kb/299357

感谢分享，世界才变得更美好。

参考资料：
1. http://social.technet.microsoft.com/Forums/windows/en-US/55aa9f24-e8ea-4743-8e04-120decfb6122/local-area-connection-doesnt-have-a-valid-ip-configuration-w7?forum=w7itpronetworking                                   