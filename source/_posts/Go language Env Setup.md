title: Go language Env Setup
date: 2015-10-08 12:46:09
tags: C/C++
---


	
		下面的文章内容基于ubuntu 14.04 实验
	
	
		因为ubuntu 默认的apt-get 安装的golang是1.1 版本，太低了，所以需要自己通过binary或者source安装。
	
	
		对于ubuntu 14.04 go 官方提供了合适的binary包，所以直接使用binary 安装
	
	
		
	
	
		binary 安装比较简单如下
	

	
		
	
	
		
	
	
		我的ubuntu系统对应的包是go1.5.1.linux-amd64.tar.gz
	
	
		
	
	
		2. 安装
	
	
		sudo tar -C /usr/local -xzf go1.5.1.linux-amd64.tar.gz
	
	
		
	
	
		3. 设置PATH和GOPATH
	
	
		vim  $HOME/.profile
	
	
		
	
	
		添加如下的内容：
	
	
		export GOPATH=$HOME/work
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
	
	
		
	
	
		4. 测试
	
	
		$ cd $GOPATH
	
	
		$ mkdir -p src/github.com/user/hello
$ cd src/github.com/user/hello

$ vim hello.go

//--------------
package main

import "fmt"

func main() {
    fmt.Printf("hello, world\n")
}

//------------
	
	
		
	
	
		
	
	
		$ go install github.com/user/hello

$ $GOPATH/bin/hello
hello, world
	
	
		
	
	
		
	
	
		
	
	
		参考：
	
	
		
	
	
		1. 
	
	
		2. 
	


		下面的文章内容基于ubuntu 14.04 实验
	
		因为ubuntu 默认的apt-get 安装的golang是1.1 版本，太低了，所以需要自己通过binary或者source安装。
	
		对于ubuntu 14.04 go 官方提供了合适的binary包，所以直接使用binary 安装
	
		
	
		binary 安装比较简单如下
	
		
	
		
	
		我的ubuntu系统对应的包是go1.5.1.linux-amd64.tar.gz
	
		
	
		2. 安装
	
		sudo tar -C /usr/local -xzf go1.5.1.linux-amd64.tar.gz
	
		
	
		3. 设置PATH和GOPATH
	
		vim  $HOME/.profile
	
		
	
		添加如下的内容：
	
		export GOPATH=$HOME/work
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
	
		
	
		4. 测试
	
		$ cd $GOPATH
	
		$ mkdir -p src/github.com/user/hello
$ cd src/github.com/user/hello

$ vim hello.go

//--------------
package main

import "fmt"

func main() {
    fmt.Printf("hello, world\n")
}

//------------
	
		
	
		
	
		$ go install github.com/user/hello

$ $GOPATH/bin/hello
hello, world
	
		
	
		
	
		
	
		参考：
	
		
	
		1. 
	
		2. 
	