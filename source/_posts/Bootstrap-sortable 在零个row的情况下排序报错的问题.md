title: Bootstrap-sortable 在零个row的情况下排序报错的问题
date: 2016-10-19 08:09:48
tags: JavaScript
---


						最近使用了 bootstrap-sortable，发现一个问题

    Uncaught TypeError: Cannot read property 'appendChild' of undefined

这个问题是在表格没有数据的情况下报的错，调查发现是 Tinysort 的问题，https://github.com/Sjeiti/TinySort/issues/102
注明：Tinysort 是一个对HTML元素进行排序的 javascript 脚本。

这个已经在 Tinysort 2.2.4中修订了，但是 bootstrap-sortable 因为长久没有及时的更新，导致同样的问题。


解决方法：在 bootstrap-sortable 项目没有更新 Tinysort 前，需要自己手动更新 Tinysort. 直接将对应版本的 Tinysort 代码更新，如
https://github.com/Sjeiti/TinySort/blob/v2.2.4/dist/tinysort.jgz
                                   