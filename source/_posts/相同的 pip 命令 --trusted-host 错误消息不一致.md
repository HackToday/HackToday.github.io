title: '相同的 pip 命令 --trusted-host 错误消息不一致'
date: 2017-10-25 18:23:55
tags: LINUX
---

问题：
最近部署系统的时候，发现 ansible 执行同样的 pip 安装命令，结果不一样，一台正常，一台报错  --trusted-host 没有设置。

原因：
简单的调查发现，其中的 pip 版本不一致，一个没有涉及到 --trusted-host， 另外的新版的 pip 支持了 --trusted-host

修复：
简单的方法是，将其中支持 --trusted-host 配置到 pip.conf 文件中