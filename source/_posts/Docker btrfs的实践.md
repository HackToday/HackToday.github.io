title: 'Docker btrfs的实践'
date: 2016-02-02 21:39:26
tags: 云计算
---

使用OpenStack部署的虚拟机，其中只有一块盘，但是docker的btrfs需要一个单独的设备，如果OpenStack没有Cinder服务，
还要手动加盘，实在有点麻烦，作为实验来说有点不便。那么怎么绕过去呢，很简单，使用全能的loop device，具体如下:

1. 创建一个空的image文件(9.6G）

```
dd if=/dev/zero of=btrfs.img bs=512 count=20000000
```

2. 建立loop，操作来为mount准备

```
losetup /dev/loop0 btrfs.img
```

3. 安装btrfs tools

```
sudo apt-get install btrfs-tools
```

4. 创建btrfs存储池

```
sudo mkfs.btrfs -f /dev/loop0
```

5. 创建docker使用的文件目录

```
sudo mkdir -p /var/lib/docker
```

6. 获取btrfs文件系统的UUID

```
sudo blkid /dev/loop
```

7. 创建对应的/etc/fstab项目，使得可以系统启动时可以自动挂载

```
/dev/loop0 /var/lib/docker btrfs defaults 0 0
UUID="b18ea60f-5cad-4b3d-8769-a2da818fdedb"  /var/lib/docker btrfs defaults 0 0
```

8. 挂载上面的新的文件系统

```
sudo mount -a
```

9. 重启docker

```
sudo service docker start
```

这样就完成btrfs storage driver配置完成。

后面我们想做一个实验，想获取btrfs文件的Magic Number : 9123683e

```
docker run --rm -it -v "$PWD":/usr/src/mygo -w /usr/src/mygo  -v /var/lib/docker:/var/lib/docker  golang bash

export GOPATH=$GOPATH:/usr
```

```
// mygo/main.go

package main

import (
        "fmt"
        "log"
        "os"
        "syscall"
)

        
func main() {
        log.Printf("Args are :%s ", os.Args)
        var stat syscall.Statfs_t
        if err := syscall.Statfs("/var/lib/docker", &stat); err != nil {
                log.Fatalf("Stat error\n")
        }
                
        fmt.Printf("The type is %x\n", uint32(stat.Type))
} 
//
```

在容器中运行程序，可以得到结果:
2016/02/02 13:38:46 Args are :[mygo] 
The type is 9123683e

参考资料:

http://wiki.osdev.org/Loopback_Device
https://www.howtoforge.com/a-beginners-guide-to-btrfs
https://github.com/docker/docker/blob/master/docs/userguide/storagedriver/btrfs-driver.md                                   