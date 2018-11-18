title: 'Golang 最新的模块特性'
date: 2018-11-18 15:54:28
tags: Golang
---

## Go Module

Go 1.11 引入对于 Module 概念的初步支持，这个特性具体是什么？ 为什么要引入这个概念，我觉得有必要给大家简单的介绍一下。

### 背景

Go 从最初的 2009.11 发布到如今，已经迈过了 9 年的时间，Go 的包版本管理经历了几个阶段性发展。

- 2009.11

最初的开始，没有确定的方法来共享 Go 发布的程序，依靠一个简单的 gobuild 封装来构建
单一包，产生 makefile 文件，这种方式会 

- 2010.2

goinstall 的提出引入了包的命名格式方式，无需特殊配置来对版本库的代码进行代码库下载和管理

- 2011.12

Go 1，引入 go 命令 go get 来代替 goinstall

go get 作为代码共享的入口，带来了很大的便利，但是有一个重要的问题没哟解决，关于不同包的代码版本如何控制和管理 go get 没有去解决。

go get 默认从代码版本工具获取最新的拷贝，引入了两个问题。 

其一，用户无法预料 go get 更新带来的问题  2013~2015
其二，无法确保重复性的生成相同的编译版本。  2012~2015

针对问题1，引入了语义化版本规范， 针对问题2，引入了不同的非官方工具，`Goven`, `godep`, `gb`，实现方式各异，无法统一。 
2016-2017 一个接近官方的工具的诞生和普及 Dep，但是仍然面临类似的问题，缺少和标准化 go 工具统一集成设计的理念。 
后来 Go 团队就通过一个新的 vgo 项目来进行后续的工作了。

go module 从 vgo 实验（2018.2) 到 1.11 正式纳入官方的实验特性 (2018.8) ，说明 Go 团队仍有许多的设计和社区反馈需要持续演进，
社区期望在 1.12 版本中 go module 能够最终稳定下来。


### 使用方法

- 对于新的项目


操作方法如下：


#### 创建目录

```
$ mkdir -p  /tmp/scratchpad/hello

$ cd /tmp/scratchpad/hello
```

#### 初始化模块

```
$ go mod init github.com/HackToday/hello

```

#### 项目代码

```
$ cat <<EOF > hello.go
package main

import (
    "fmt"
    "rsc.io/quote"
)

func main() {
    fmt.Println(quote.Hello())
}
```

#### 运行，生产相应的 mod 文件

```
$ go build 
$ ./hello

Hello, world.
```

```
$ cat go.mod

module github.com/you/hello

require rsc.io/quote v1.5.2
```

- 对于已有的项目

#### 项目的结构

```
$ cat main.go

package main

import (
	"fmt"
	"github.com/go-redis/redis"
)

func main() {
	fmt.Println("vim-go")
	client := redis.NewClient(&redis.Options{
		Addr:     "localhost:6379",
		Password: "",
		DB:       0,
	})

	pong, err := client.Ping().Result()

	fmt.Println(pong, err)

```

#### 根据依赖生成 mod 文件

```
$ go mod init github.com/HackToday/mtest
```

*注明：* 直接运行 `go mod init` 会报错，是因为无法自动推导出模块路径

```
$ cat go.mod
module github.com/HackToday/mtest
```

#### 构建模块

```
$ go build ./...
```

```
$ cat go.mod 
module github.com/HackToday/mtest

require github.com/go-redis/redis v6.14.2+incompatible

```
#### 测试验证

```
$ go test ./...

$ go test all
```

### 参考文献

https://github.com/golang/go/wiki/Modules
https://semver.org/lang/zh-CN/
https://go.googlesource.com/proposal/+/master/design/24301-versioned-go.md