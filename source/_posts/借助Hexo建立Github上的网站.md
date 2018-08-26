title: '借助Hexo建立Github上的网站'
date: 2016-04-29 22:46:08
tags: 云计算
---

上一篇Hexo中，我们介绍了它是一个简单提供了快速搭建静态博客系统的软件，那么我们就想借助它来实现可以建立一个公开可访问博客网站，
问题来了，如果没有自己的域名服务器，怎么实现这个要求？

有人想到了使用公有云的虚拟机，但是域名购买还是需要的，使用ip访问？这个显然不太方便让外面用户访问，这时我们想到了github pages。

Github提供了github pages功能，借助它可以方便的实现个人博客的在线系统。

那具体操作如下：（下面的操作都是在上一篇启动的Hexo容器中操作）

1. 建立一个公共的repository， username.github.io
2. git clone到本地，cd 到相应的目录，建立一个新的分支，这里我们使用的名字是source，下面所有的操作都是source分支
3. 执行hexo init初始化，生成相应的博客系统配置文件
4. 执行 npm install安装相应的nodejs包
5. 执行 npm install hexo-deployer-git --save，这一步配置完成hexo git deploy插件的安装
6. 配置相应的git deploy 部署参数：
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: https://github.com/username/username.github.io.git
  branch: master

7. 然后在source文件夹写博客
8. 返回到主目录，执行hexo g，产生相应的博客网站需要的文件
9. 执行hexo deploy 部署博客网站
访问 http://username.github.io/， 就是部署后的网站了。

这里借助github.io的repository就方便的实现了博客源文件管理（source分支）和快速部署（master分支）的需求，
而且借助github pages实现了在线博客的运行。

参考：

1. https://pages.github.com/
2. https://github.com/hexojs/hexo
                                   