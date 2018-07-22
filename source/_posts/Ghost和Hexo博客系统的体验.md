title: Ghost和Hexo博客系统的体验
date: 2016-04-19 16:31:55
tags: LINUX
---


						一直打算部署博客系统，比较了流行的WP, Ghost, Hexo and Pelican

网上先调研了一番，发现有两篇文章比较有参考价值：
一个是个人的实践 http://www.maintao.com/2014/hexo-beginner%27s-guide/
另外一个是知乎的 http://www.zhihu.com/question/21981094 


综合比较下来，发现还是这个Hexo满足我的最简单最小化静态博客需求，但是既然借助容器体验非常方便，那么我就也顺便体验了ghost一番

Hexo使用：
1.安装Hexo
   docker run --name hexo -it -p 8083:80 -v `pwd`: /usr/share/nginx/html/source simplyintricate/hexo
2. 发布一篇blog
docker exec -it hexo hexo new "This is one post"


Ghost使用
 1. docker run --name some-ghost -p 8085:2368 -d ghost
直接操作UI就行了

Ghost相对UI比较好，操作简单，但是依赖数据库存储，而且现有的API发布一篇markdown文档不简单

Hexo简单，UI稍微逊色，不依赖数据库，原始文档即博客数据（存在固定的文件夹下），非常方便，发布新的博客，需要重启容器，这点网上也介绍有第三方工具，实现动态更新，还没有研究。

                                   