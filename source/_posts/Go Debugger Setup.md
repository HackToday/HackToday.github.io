title: 'Go Debugger Setup'
date: 2015-10-08 12:47:07
tags: C/C++
---

Go官方没有指定的debugger， gdb因为不能很好的调试go程序，gdb对go的支持已经不再维护，参见

```
GDB does not understand Go programs well. The stack management, threading, and runtime contain aspects that differ enough from the execution model GDB expects that they can confuse the debugger, even when the program is compiled with gccgo. As a consequence, although GDB can be useful in some situations, it is not a reliable debugger for Go programs, particularly heavily concurrent ones. Moreover, it is not a priority for the Go project to address these issues, which are difficult. In short, the instructions below should be taken only as a guide to how to use GDB when it works, not as a guarantee of success.

In time, a more Go-centric debugging architecture may be required.
```

IRC问了一个go developer， 推荐 delve 或者使用fmt.Println() 
下面尝试使用delve，

1. Go Env 安装

确保你本地的环境已经安装了Go并且配置GOPATH，参见上一篇我的Go Env Setup

2. Delve 安装

```
$ git clone https://github.com/derekparker/delve.git  $GOPATH/src/github.com/derekparker/delve


$ go get github.com/peterh/liner
$ go get github.com/spf13/cobra
$ go get golang.org/x/sys/unix
$ go get gopkg.in/yaml.v2
```


上面的get命令是安装相应的package。这事因为在make install的会报错，提示上面的package找不到

```
$ make install
```

3. 调试Go程序

```
$ dlv exec hello

(dlv) break hello.go:6

(dlv) continue
> main.main() ./hello.go:6
    
     1:     package main
     2:    
     3:     import "fmt"
     4:    
     5:     func main() {
=> 6:         i := 12
     7:         j := 13
     8:         t := i + j
     9:         fmt.Printf("hello, world\n")
    10:         fmt.Println(i, j, t)
    11:     }

(dlv) n

(dlv) print i
1
```

参考：

1. https://github.com/derekparker/delve/wiki/Building
2. https://golang.org/doc/gdb
3. http://geekmonkey.org/2012/09/comparison-of-ides-for-google-go/
4. https://github.com/derekparker/delve/wiki/Building