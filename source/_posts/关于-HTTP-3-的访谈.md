title: 关于 HTTP/3 的访谈
date: 2019-06-30 17:05:48
tags: 互联网
---

### HTTP/3 是什么？

HTTP3 是为了解决现有的 HTTP2 一些性能，安全上的问题而提出的，最初的原型来自于 Google 的内部
QUIC 传输层协议，基于 UDP 开发。 

HTTP2 历经了 16 年洗礼，才逐渐被各大浏览器产商，软件厂商接受和应用起来， HTTP2 的座位还没稳固，
HTTP3 就已经开始迈向历史舞台， 发展过程大致如下：

```
QUIC （2012 Google） --》  IETF 进行标准化（2015）  --》 QUIC（核心规范提交 IESG  2019）
```

### HTTP/3 做了什么改进？

HTTP/2 相比 HTTP /1.1 最大程度解决了 HTTP head of line blocking， 引入了 Multiplexing 方法，
通过一个 TCP 连接就可以实现客户端，服务端之间的双向数据流并行发送，带来了一定条件下的性能提升。


HTTP/2 确实大大减少了 TCP 连接数，但是却没有解决 TCP 协议层面的引发的 head of line blocking，
这个主要是 TCP 本身是有序传输的，单个流的数据丢失会导致其他的数据流受影响而等待，直到丢失的数据
被重传接收。 这个问题在丢包率较高的环境下，会让 HTTP/2 相对于的 HTTP/1.1 的性能优势荡然全无。 

为了克服 TCP 协议的这个固有问题，QUIC 引入了基于 UDP 之上的类 TCP 实现，使用独立的数据流进行并行传输，
单个流的数据丢失只是影响自身，其他的数据流不必停下来等待。

至于为什么选择 UDP， 而不是改进 TCP 或者设计一个新的协议，这个还是历史包袱的问题，大量的网络基础设施
已经遍布世界各地，更换代价和时间成本很高，全新的协议基本不靠谱；TCP 协议又因为被广泛使用，改动变更和
落实也是很头疼的事情，基本是出力不讨好。 为了让改进特性更快的尝试和兼容，最终选择了基于 UDP 进行传输，
同时上层的放在用户态空间实现，减少了对不同操作系统内核的过度依赖。

###  HTTP/3 协议栈

下面的这个图形象的说明了 HTTP/3 相对 HTTP/2 协议栈的不同：

{% asset_img HTTP3.png This is http3 stack image %}

来源：https://http3-explained.haxx.se/zh/the-protocol.html

### HTTP/3 的未来

尽管 HTTP/3 已经在标准化的路上走得越来越稳，摆在面前的一些质疑和问题依然不断，主要是

- UDP 的性能
- QUIC 资源占用
- 成长太慢

### 参考资料

1. https://datatracker.ietf.org/wg/quic/charter/
2. https://http3-explained.haxx.se/zh/why-h2.html
3. https://tools.ietf.org/html/draft-ietf-quic-transport-18
4. https://tools.ietf.org/html/draft-ietf-quic-transport-20
5. https://diophant.com/blog/what-is-http3-and-why-is-it-needed/

