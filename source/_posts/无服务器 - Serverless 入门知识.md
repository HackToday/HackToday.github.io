title: '无服务器 - Serverless 入门知识'
date: 2018-02-25 13:02:19
tags: 云计算
---

Serverless 这个单词最早大约来源于Ken Form 的一篇文章中， 不过并不是今天我们要讨论的这个意思，如今的含义是无服务器，主要是指让开发人员专注于业务代码的本身逻辑上，无需关注代码部署的资源、维护、扩展性和高可用性，减少代码开发人员的业务无关的工作量，提高开发效率，具有无状态运行，基于事件触发，按需付费，运维完全托管到第三方等特点。

<!-- more -->
Serverless 面向的粒度主要是应用层面，更具体说是应用的函数层面，在 XaaS 里面，更偏向是 FaaS，但是仅仅 FaaS 也不全面，因为Serverless 的这种分发和使用方式，开发者更多的时候是使用第三方提供的 API 来封装调用集成服务，所以也包含了 BaaS （Backend as a Service）的范畴，其中 FaaS 是最突出的一个特点，一方面，函数级别的粒度，必然对于运行时间，计费方式有新的要求和设计，另外一方面，对于如何快速开发出符合平台规范的 FaaS  ，以及跨平台的 FaaS 方法论上存在很多问题尚未很好的解决。

FaaS 目前在各大云厂商都有了相应的支持，支持的程度略微不同，主要包括，语音运行环境，开发调试工具，第三方云服务集成，触发方式

Serverless 作为一种新型的软件架构和概念，可以和几个有意思的概念相比较来说明：

微服务，  也是一种软件低耦合，分布式，模块化界限设计软件架构方式，微服务也是面向应用，不过更多的是从组件或者模块化的粒度来设计分析

Serverless， 如今更多的是面向 Function， 虽然 Function 粒度有大小区分，但是执行时间的粒度和生命周期方式和微服务有很多不同

PaaS， 作为面向应用的一个平台服务，更多的是对 IaaS 层面的封装和抽象，伴随着容器化的流行，私有 PaaS 和 公有 PaaS 都在不同的企业中有很多落地实践，PaaS 很多的设计依然没有做到完全透明，比如实例的数量，资源的需求，以及扩容的配置等。相比 FaaS ，透明性不高，控制能力稍强。

Serverless 这个概念开始火大约来源于 AWS 2014 年的发布的 Lambda，用户仅需对自身业务代码负责，运行即产生费用，无须关注机器资源，这样的计算抽象间接的引入一种新的应用发布形式，


注明：其他产商的 Serverless 功能都大体类似，这里仅仅以 lambda 为例

对于这种基于 Function 粒度新型的开发模式，势必一个值得研究的问题就是开发效率的问题，如何更加方便的开发，分发这些 function ，如何保证 function 的安全性，便是最近 AWS 官方发布的 Serverless Application Repository，每个案例都是一个可以部署试验的应用，这些应用程序都是采用 SAM 模板方式分发，保证了 Serveless 应用的可复制性和可分发性。因为 Function 粒度的问题，还有应用的可操作性，使用性，所以这个仓库中的范畴，从代码，组件到应用都包含，间接的可以理解为一个容器仓库，只不过容器仓库本身面向的是容器应用而已。


可以预见未来很长的时间， AWS 应该为着力把这个应用仓库完善起来，只有仓库丰富性增强，相应的 Serverless 生态才可能壮大，才会有更多应用落地，方便开发，降低学习 Serverless 的门槛。当然如何类似 Docker 仓库，实现基于 dockerfile 更简易的编程和继承方式也是值得研究的地方。

最后总结一下 Serverless 未来学习需要考虑的几个点：

语言平台支持维度
开发调试工具支持
标准互通，第三方厂商绑定，
编程，维护性支持（编排）
函数性仓库支撑
可控延迟性，性能支撑迁移成本
监控，诊断
和已有的应用集成难度

参考资料：
https://en.wikipedia.org/wiki/Serverless_computing
http://serverless.ink/
https://jimmysong.io/posts/what-is-serverless/
https://amazonaws-china.com/cn/blogs/china/iaas-faas-serverless/
https://amazonaws-china.com/cn/serverless/
https://amazonaws-china.com/cn/lambda/
https://amazonaws-china.com/cn/serverless/serverlessrepo/
http://fastzhong.com/2018/01/30/serverless%20-%20intro/
https://cloud.google.com/serverless/
http://gitbook.cn/books/5a4b66e3f957ee33939ec3cf/index.html
https://s3.cn-north-1.amazonaws.com.cn/videos-localize/%E5%BF%AB%E9%80%9F%E7%90%86%E8%A7%A3AWS+Lambda%EF%BC%8C%E8%BD%BB%E6%9D%BE%E6%9E%84%E5%BB%BAServerless%E5%90%8E%E5%8F%B0.pdf
