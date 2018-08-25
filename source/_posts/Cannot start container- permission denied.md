title: 'Cannot start container: permission denied'
date: 2015-02-10 15:37:52
tags: 云计算
---

在前一篇我们讨论的kubernetes fedora Ansible环境建立篇中，在fedora 20环境中如果运行

```
cat ~/apache.json
{
  "id": "fedoraapache",
  "kind": "Pod",
  "apiVersion": "v1beta1",
  "desiredState": {
    "manifest": {
      "version": "v1beta1",
      "id": "fedoraapache",
      "containers": [{
        "name": "fedoraapache",
        "image": "fedora/apache",
        "ports": [{
          "containerPort": 80,
          "hostPort": 80
        }]
      }]
    }
  },
  "labels": {
    "name": "fedoraapache"
  }
}
```

```
kubectl create -f apache.json
```

会发现minion的log中包含了Cannot start container: permission denied信息，其实这个是selinux一个相关的bug

https://ask.fedoraproject.org/en/question/50871/cant-run-docker-without-privileged-on-fedora-20/

解决方法：

方法1: 采用其他支持selinux的docker image
方法2：在minion节点setenforce 0，然后重新运行kubectl命令即可

本文采用了方法2，结果如下：

master节点检查

```
$ kubectl get pod fedoraapache
NAME                IMAGE(S)            HOST                LABELS              STATUS
fedoraapache        fedora/apache       a.b.c.d/        name=fedoraapache   Running
```

minion节点检查

```
$ curl http://localhost
Apache
```