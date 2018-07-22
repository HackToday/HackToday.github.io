title: Redhat resolv.conf changes disappear!
date: 2015-07-20 09:50:39
tags: LINUX
---


						最近使用redhat 7.1 系统，改过resolv.conf，总是发现文件修改完，重启系统，文件又变成没有修改之前的状态了。

原来查出是NetworkManager在作怪。

http://totalcae.com/blog/2013/06/prevent-etcresolv-conf-from-being-blown-away-by-rhelcentos-after-customizing/                                   