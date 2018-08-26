title: 'marathon 1.3.0 功能介绍'
date: 2016-10-24 17:51:23
tags: 云计算
---

marathon 1.3.0 相较1.1.0 增加一些新的功能，同时有些新功能的加入并不是向后兼容的，包括对外部的依赖
marathon 从 1.2.0 版本以后，就开始要求 mesos 的版本大于1.0.0

不兼容修改：

下面来详细介绍一下新的功能：

1. 新的选举依赖库

marathon 的 leader 选举代码改进增加了稳定性，Leader 选举的代码是依赖于 Curator 第三方库的
因为新的leader 选举库是和 1.2.0 之前marathon 不兼容的，所以如果从老的 marathon 升级而来，
需要停掉老的marathon 再进行升级，否则会出现多个 leader 冲突

2. Framework 的增加了验证命令行参数支持

Framework 的认证主要是为了配合 mesos 认证功能来使用的，mesos 的认证运行可信赖的 Framework 向 mesos 注册。
默认是关闭的，用户需要通过  --mesos_authentication 来使用 这项功能

3. TASK_LOST GC 的超时默认值的更改

如果 task 确认为lost 状态，因为某个原因表明 task 还会恢复，marathon 等待其恢复直到超时
原来默认的是 24 小时，现在改为 75秒，可以通过以下的参数来控制

```
-task_lost_expunge_gc, --task_lost_expunge_initial_delay, --task_lost_expunge_interval.
```

新功能简介

Universal Containerizer
从 1.3.0开始，marathon 支持在没有docker daemon 的情况下部署docker image。
通过原生的操作系统的功能来支持 AppC 和 docker 镜像启动和配置，提供隔离性

TASK_LOST behavior
如果mesos agent 和 master 失联，所有agent 上的 task 都认为是 lost 状态。 marathon 过去是对这些 lost task直接杀掉的。
但是某些情况下，这些agent 可能会重新加入集群，所以 lost并不是终止态

在这个新版本，marathon 会等待  lost task 直到被确认为 dead，默认超时时间是 75 秒，超过75秒，lost task 就被杀掉了


Task Kill Grace Period
每个应用都可以定义个kill grace period，当对task 进行杀死操作的时候，agent 会尽量在这个grace period之后销毁task。
但是这个等待时间是弱限制的，agent 可能会给其分配一个短的间隔强制终止

MAX_PER constraint
之前版本的限制无法满足在某个agent 部署固定数目的容器，现在可以限制部署任意的数量

Virtual heartbeat monitor
之前版本的marathon 在网络改变的情况下，无法知道和master失联，虚拟心跳提供了这种支持

Authorization  to system endpoints
之前版本的marathon包含了 对于 AppDefinition 和 Group 改变的授权钩子，新的版本增加了  /v2/leader, /v2/info, /v2/events , /v2/eventSubscriptions
 授权钩子

Secrets API support
在AppDefinition 中可以使用secrets了，secrets 作为重要的实体在环境变量里使用
原生的mesos不支持，需要借助一个插件来支持

Support for Nvidia GPU
在AppDefintion 中可以使用gpus作为Nvidia GPU资源了，mesos 对gpus这种重要实体提供原生支持，marathon 中 通过命令行参数
 `--enable_features gpu_resources`来开启使用，要想使用上面的功能，mesos 还需要编译了包含了对于  Nvidia GPU的支持

Support all attribute types with constraints
新版中对非文本类型的属性提供了constraint支持

zookeeper digest authentication support
zk 客户端现在可以支持 zk 认证 和 acl了

Support for virtual networking for docker containers
提供了Docker USER 网络的支持

CNI networking support
增了ipAddress.Name的可选字段，可以使用CNI 网络来启动task

Enforce the uniqueness of service ports
marathon 会保证所有新创建或者更新的app 的service端口是新的
这个可能导致marathon原来使用的 app defintion 失效

若干的性能改进和缺陷修正

注明：文中如有理解偏差之处，欢迎指正。