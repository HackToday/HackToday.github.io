title: 'devstack 的 glance-api启动失败的原因'
date: 2013-06-20 17:46:19
tags: 云计算
---

devstack 更新了一下，在recllone的时候，发现有个现象 glance-api 无法响应请求。

经过调查发现，原来自己的在glance文件目录下创建过bin （glance 从havana取消bin目录了）当时是import 到
eclipse工程中用来调试用的。 devstack reclone 不会删除新创建的目录，所以导致glance-manage db_sync失败，
导致没有数据库glance。所以api请求导致meta query失败，500错误。
