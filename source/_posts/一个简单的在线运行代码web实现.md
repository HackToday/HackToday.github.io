title: 一个简单的在线运行代码web实现
date: 2015-05-05 13:44:47
tags: Html/Css
---


						前几天想实验一个在线运行code的web实现，在github商店找到一个简单的事例，clone如下，
https://github.com/HackToday/codelauncher


实际上，这个项目最初的实现支持包括c和python，使用Flask Web框架来进行开发，使用了模版继承等特性，
具体参见这个 http://flask.pocoo.org/docs/0.10/quickstart/


我们可以简单的添加其他语言的支持，比如ruby，go 等。
以Ruby为例，简单的添加代码如下：

https://github.com/HackToday/codelauncher/commit/aa912a1303460bed35537d04b027fee846c0697f
https://github.com/HackToday/codelauncher/commit/879a10e37362218a61cdb6bae259b58c26ef2ba6


这个简单的实现，可以在此基础上做点有意思的事情，大家可以实验一下。                                   