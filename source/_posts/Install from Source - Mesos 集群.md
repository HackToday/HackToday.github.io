title: 'Install from Source - Mesos 集群'
date: 2016-09-12 21:45:36
tags: 云计算
---

1. 安装build和运行环境依赖

```
# Install the latest OpenJDK.
$ sudo apt-get install -y openjdk-8-jdk

# Install autotools (Only necessary if building from git repository).
$ sudo apt-get install -y autoconf libtool

# Install other Mesos dependencies.
$ sudo apt-get -y install build-essential python-dev libcurl4-nss-dev libsasl2-dev libsasl2-modules maven libapr1-dev libsvn-dev zlib1g-dev
```

2. 编译 mesos 

```
# Change working directory.
$ cd mesos

# Bootstrap (Only required if building from git repository).
$ ./bootstrap

# Configure and build.
$ mkdir build
$ cd build
$ ../configure
$ make
```

其中遇到的问题
对于找不到jar的处理： http://www.linuser.com/thread-2057-1-1.html

3.  生成运行框架（例子）

```
# Run test suite.
$ make check

4. 运行mesos
$ cd build

# Start mesos master (Ensure work directory exists and has proper permissions).
$ sudo ./bin/mesos-master.sh --ip=127.0.0.1 --work_dir=/var/lib/mesos

# Start mesos agent (Ensure work directory exists and has proper permissions).
$ sudo  ./bin/mesos-agent.sh --master=127.0.0.1:5050 --work_dir=/var/lib/mesos
```

安装/运行 zookeeper 

1. 下载安装包 （其实这一步是不需要的，因为mesos的build 中集成了zookeeper，
参见 https://mesosphere.com/blog/2013/08/01/distributed-fault-tolerant-framework-apache-mesos-html/
其中的介绍， 可以直接进入 /build/3rdparty/zookeeper-3.4.*, 调到步骤2）

```
  wget http://apache.fayea.com/zookeeper/stable/zookeeper-3.4.9.tar.gz
```

2. 创建 conf/zoo.cfg

```
tickTime=2000
dataDir=/var/lib/zookeeper
clientPort=2181
```

3. 启动zookeeper in standalone mode

```
$ bin/zkServer.sh start
```

4. 验证zookeeper 可以正常工作

```
$ bin/zkCli.sh -server 127.0.0.1:2181
```

运行 help , ls / 命令等


对于安装zookeeper 后，mesos需要向zookeeper注册，所以原来的mesos 启动的命令需要修改如下：

```
$ ./bin/mesos-master.sh --zk=zk://localhost:2181/mesos    --work_dir=/var/lib/mesos  --quorum=1
$ ./bin/mesos-agent.sh --master=zk://localhost:2181/mesos  --work_dir=/var/lib/mesos
```

应用docker containerizers，为了使用docker 容器化，我们需要使用 --containerizers=docker,mesos
主要是 agent 启动参数修改，具体如下：

```
sudo ./bin/mesos-agent.sh --master=zk://localhost:2181/mesos  --work_dir=/var/lib/mesos --containerizers=docker,mesos
```
因为agent 修改了，启动时候可能报错，执行以下 `sudo rm -rf /var/lib/mesos/meta/slaves/latest` 就行了


安装/运行 marathon

1.  安装有两种方式，一种是源码安装，另外一个是release tarball 安装

```
   - option1：build  from  source
         https://github.com/mesosphere/marathon
   - option 2:  tarball install
        wget http://downloads.mesosphere.com/marathon/v1.1.1/marathon-1.1.1.tgz
```

2. 启动marathon

```
     $ sudo MESOS_NATIVE_JAVA_LIBRARY=/home/kennan/mesos-1.0.1/build/src/.libs/libmesos-1.0.1.so ./bin/start  --master zk://localhost:2181/mesos   --zk zk://localhost:2181/marathon
```

如果mesos没有注册到zookeeper 也就是 standalone mode 方式，比如 local 方式 或者 http://host:port ，命令如下：

```
$ sudo MESOS_NATIVE_JAVA_LIBRARY=/home/kennan/mesos-1.0.1/build/src/.libs/libmesos-1.0.1.so ./bin/start  --master local  --zk zk://localhost:2181/marathon
```
使用Unified Containerizer
为了减少对docker dameon的依赖，mesos引入了unified containerizer，通过对docker registry 的curl请求， 内部的isolators需要开启来支持对应的隔离。（取代docker的对于容器创建隔离的部分）。所以，为了使用这个功能，相应的agent运行的命令修改如下：

```
sudo ./bin/mesos-agent.sh --master=zk://localhost:2181/mesos  --work_dir=/var/lib/mesos --containerizers=mesos --image_providers=appc,docker --isolation=filesystem/linux,docker/runtime
```

测试：

```
/mesos-1.0.1/build/src$ sudo ./mesos-execute  --master=localhost:5050   --name=test   --docker_image=library/redis  --shell=false

$ sudo docker run -ti --net=host redis redis-cli
127.0.0.1:6379> ping
PONG
127.0.0.1:6379>
```

比如，我们运行nginx，不在使用默认image里的entrypoint和cmd，而是这里自己制定：

```
sudo ./mesos-execute  --master=localhost:5050   --name=test2   --docker_image=library/nginx  --shell=true --command="nginx -g 'daemon off;'"
```

访问 http://127.0.0.1/ 就可以看到nginx 页面了

更多参考资料：

1. http://mesos.apache.org/documentation/latest/operational-guide/
2. mesos containerizers
   http://mesos.apache.org/documentation/latest/containerizer/
3. zookeeper 介绍
   http://zookeeper.apache.org/doc/trunk/zookeeperStarted.html
4. Unified Containerizer
   https://github.com/apache/mesos/blob/master/docs/container-image.md