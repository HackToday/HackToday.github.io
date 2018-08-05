title: 'Redhat 7的kernel新看点'
date: 2014-06-22 11:38:07
tags: 云计算
---

Redhat 2014的summit关于RHEL 的roadmap中，关于kernel的部分，讲了很有意思的一些新特点，
主要包括，架构，内存管理，调度和锁机制的改进，性能提升，debug，还有container....

```
RHEL 7 Kernel Architecture Support
1） Architectures
Support the following 64 bits Architectures
X86_64, Power, and s390
with 32bit user space compatibility support
2） Theoretical Limits on X86_64
–Logical CPU – maximum 5120 logical CPUs
–Memory – maximum 64T


Resource Management Improvements
1）Linux Containers (LXC) – Fully Supported in RHEL 7 RC
2）Control Groups: cpu, cpuset, memory, block io, network, network prio
3）Libcgroup has been deprecated, replacing with systemd's scope and slices
4）Namespaces: mount, UTS, IPC, PID, network
5）User Namespace – in later releases
6）SELinux – security protection for containers
7）SystemD – provide unit file to help setup container’s resources
8）Docker CLI 
```

其实container才是我感兴趣的地方，Docker公司最近发布的1.0可谓在业界风生水起，

```
	What is Docker's architecture?

	Docker uses a client-server architecture. The Docker client talks to the
Docker daemon, which does the heavy lifting of building, running, and
distributing your Docker containers. Both the Docker client and the daemon can run on the same system, or you can connect a Docker client to a remote Docker
daemon. The Docker client and service communicate via sockets or through a
RESTful API.


	The Docker daemon

	As shown in the diagram above, the Docker daemon runs on a host machine. The
user does not directly interact with the daemon, but instead through the Docker
client.


	The Docker client

	The Docker client, in the form of the docker binary, is the primary user
interface to Docker. It accepts commands from the user and communicates back and
forth with a Docker daemon.

	Inside Docker

	To understand Docker's internals, you need to know about three components:
		- Docker images.
		- Docker registries.
		- Docker containers.
```	

参考资料：

1. http://docs.docker.com/introduction/understanding-docker/#what-is-dockers-architecture
2. Redhat 2014 Summit                                   