title: 'refresh页面，如何保持当前的tab'
date: 2016-11-24 08:40:05
tags: Web开发
---

经过调查和尝试, 发现有两类方法，一个是使用location.hash，另外一种是使用localStorage

1. windows.location.hash 有一个问题，在于不切换tab的情况下，tab标号不变导致 submit 是不进行相应页面reload的

2. localStorage 也有一个潜在的问题是 对应的tab的情况下，因为是相当于cache机制，所以会造成对应的共享问题，
但是这个可以通过让每个对象公用一个 tab，减少不同对象之间的冲突


参考资料：

http://stackoverflow.com/questions/16808205/keep-the-current-tab-active-with-twitter-bootstrap-after-a-page-reload
