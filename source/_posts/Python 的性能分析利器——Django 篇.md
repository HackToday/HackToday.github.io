title: 'Python 的性能分析利器——Django 篇'
date: 2017-08-23 08:14:28
tags: Python/Ruby
---

1） django debug_toolbar  这个插件可以使用，按照官方文档配置，https://django-debug-toolbar.readthedocs.io/en/stable/installation.html
但是最主要的一点是需要配置 INTERNAL_IPS ，对于这个是需要特别注意的，假设你是使用远程调试，这个时候需要把你的客户端的 ip 加入进去，这样启动的时候，就能看到 debug panel 了，否则就会不能显示出来，这个问题也是困扰我了半天，推荐一般 debug panel 不能正常显示的时候，查看这个是否正确设置。

特点，jazzband 开发， 可以看到相应页面的 sql query 数量 和 相应的时间，还有浏览器渲染等。

2）silk 也是 jazzband 开发，针对 django，配置更加简单，除了包含上面的一些统计数据，同时可以针对某块代码，进行 profiling。

3）python 自身标准库的profiler 支持，统计的数据，可以使用 pstats 模块进行格式化输出

3.1 cprofile： 基于 lsprof 模块， 以 C 扩展的方式实现，相对开销较小，可以针对对应的代码进行 profile
3.2 profile： python 的实现，接口和 cprofile 一致，开销较大，不过第三方开发基于此较容易

3.1 和 3.2 分别有接口 enable disable 对某块代码进行 profile，设置对应的是否对 built-in 进行 profile

以 cprofile 为例，输出的内容每列信息如下：

ncalls --- for the number of calls,
tottime --- for the total time spent in the given function (and excluding time made in calls to sub-functions)
percall --- is the quotient of tottime divided by ncalls
cumtime --- is the cumulative time spent in this and all subfunctions (from invocation till exit). This figure is accurate even for recursive functions.
percall --- is the quotient of cumtime divided by primitive calls
filename:lineno(function) --- provides the respective data of each function

类似如下图：


相对的统计信息比较繁多，包含了很多 built-in 

3.3 hotshot：  (不再维护的）的方式， 进行代码级别的profile， 类似 https://code.djangoproject.com/wiki/ProfilingDjango 示例1
