title: 'tcpdump 调试 TCP 连接超时问题'
date: 2017-12-15 08:56:02
tags: LINUX
---

问题简单描述： 服务端负载均衡 4 层有超时配置，如果没有数据交互，那么 tcp 连接会断开。

为了支持可定制化配置，需要验证超时生效的问题，于是我做了一个简单的 Flask app 的镜像(启动在8083 端口, 120 秒返回结果），启动后，
使用 curl http://ip:8083 发现， 过了超时时间后，服务端仍然完整了返回了结果。


调查：这种问题，一般用 tcpdump 跟踪一下就容易解决。

```
08:19:59.630162 IP myhost.24435 > VIP.http: Flags [P.], seq 1:77, ack 1, win 58, length 76
08:19:59.721043 IP VIP.http > myhost.24435: Flags [.], ack 77, win 58, length 0

08:20:59.720669 IP myhost.24435 > VIP.http: Flags [.], ack 1, win 58, length 0
08:20:59.811523 IP VIP.http > myhost.24435: Flags [.], ack 77, win 58, length 0
08:21:59.819678 IP myhost.24435 > VIP.http: Flags [.], ack 1, win 58, length 0
08:21:59.822046 IP VIP.http > myhost.24435: Flags [P.], seq 1:18, ack 77, win 58, length 17
```

可以看到在 60 秒，myhost 发送了一个类似心跳的消息，这就是为什么超时后，连接仍然没有端口的原因了。

这个问题显然是 curl 程序本身的设置，查 curl 文档 https://curl.haxx.se/docs/manpage.html 发现

```
--keepalive-time

This option sets the time a connection needs to remain idle before sending keepalive probes and the time between individual keepalive probes. It is currently effective on operating systems offering the TCP_KEEPIDLE and TCP_KEEPINTVL socket options (meaning Linux, recent AIX, HP-UX and more). This option has no effect if --no-keepalive is used.

If this option is used several times, the last one will be used. If unspecified, the option defaults to 60 seconds.

```

原因找到了，我们再验证一下，是否是这个导致，验证如下：

- 设置一个小于 4层默认超时时间，但是大于 curl 本身的超时时间，这里设置 70s，

curl -v --keepalive-time 70  -X GET http://ip:8083

tcpdump 如下：

```
08:25:46.112532 IP myhost.31425 > VIP.http: Flags [.], ack 1, win 58, length 0
08:25:46.112635 IP myhost.31425 > VIP.http: Flags [P.], seq 1:77, ack 1, win 58, length 76
08:25:46.204289 IP VIP.http > myhost.31425: Flags [.], ack 77, win 58, length 0

08:26:56.267663 IP myhost.31425 > VIP.http: Flags [.], ack 1, win 58, length 0
08:26:56.359620 IP VIP.http > myhost.31425: Flags [.], ack 77, win 58, length 0

08:27:46.218753 IP VIP.http > myhost.31425: Flags [P.], seq 1:18, ack 77, win 58, length 17
08:27:46.218770 IP myhost.31425 > VIP.http: Flags [.], ack 18, win 58, length 0
```

没有问题，70s 发送了心跳

- 设置一个大于 4 层超时时间的配置，但是小于服务端程序本身的响应时间，这里设置为 100 秒

curl -v --keepalive-time 100  -X GET http://ip:8083

tcpdump 如下：

```
08:48:44.235749 IP myhost.64757 > VIP.http: Flags [.], ack 1, win 58, length 0
08:48:44.235823 IP myhost.64757 > VIP.http: Flags [P.], seq 1:77, ack 1, win 58, length 76
08:48:44.325588 IP VIP.http > myhost.64757: Flags [.], ack 77, win 58, length 0

08:50:14.324860 IP VIP.http > myhost.64757: Flags [R], seq 2189599717, win 0, length 0
```

收到了 Rest 包，没有问题


经过上面的分析，我们就明白了，验证问题本身，使用的测试程序不同，直接结果可能表现不一样，
这需要我们深入到测试程序本身，搞明白其中相关的设置，避免不必要的误解。

最后，上面如果用 telnet 测试，就不会衍生出上面的这个问题了，因为 telnet 默认没有相关的心跳维护，自然不会出现 curl 上面的问题了。
