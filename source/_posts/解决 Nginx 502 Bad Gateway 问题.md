title: '解决 Nginx 502 Bad Gateway 问题'
date: 2017-11-29 23:26:38
tags: 架构设计与优化
---

今天访问镜像页面的时候发现时不时的出现 502 这个奇怪的错误，感觉是个值得研究的性能相关的问题，尤其是大量镜像访问的时候出现的频率加大，
于是撸起袖子准备搞起。

环境： Nginx + gunicorn + Web App

解决思路：

1. 既然是 Nginx 问题，先看 nginx error 日志，发现

Upstream prematurely closed connection while reading response header from upstream

网上这个问题一大堆，众说纷纭。其中一个主要的线索是 Nginx 和 gunicorn 之间的连接出了问题。

通过 tcpdump 看，确实是有 Fin 信号，那么查看 gunicorn 进程的日志

2. gunicorn 显示 WORKER TIMEOUT 退出，OK 线索进一步了，超时相关的问题。
进一步看 gunicorn 文档，如果应用超过 30s， 仍然没有获得响应，自动退出。

3. 验证，发现确实 30s，程序退出。

4. 解决方法： 增大延迟，30s  --》 60s

5. 意外： 注意 60s 虽然减缓了 502 的问题，但是你会发现 504 的 Timeout 问题，这是 Nginx 和 gunicorn 之间的连接超过默认 60s， 
就会报错， nginx 里面的 proxy_read_timeout 可以配置这个参数。

6. 当然最终问题，仍然还是后台的程序有性能瓶颈，这是后续的 Web App 自身的优化了。基本可以告一段落前面的调查结果了。
   至少不会出现 502， 504 这些相关的错误了。
