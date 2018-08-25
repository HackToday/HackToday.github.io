title: 'Kubernetes多节点环境（Fedora Ansible）'
date: 2015-02-10 14:20:08
tags: 云计算
---

昨天参照 https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/fedora/fedora_ansible_config.md官方的说明老是碰到一个问题：

```
$ sudo systemctl start kubelet
Failed to issue method call: Unit docker.socket failed to load: No such file or directory.
```

Kubernetes社区和Redhat bugzilla说明了是因为package的缘故，旧的依赖是docker.socket，需要更新为新的依赖docker.service （黑体部分）

```
$ cat /usr/lib/systemd/system/kubelet.service
[Unit]
Description=Kubernetes Kubelet Server
Documentation=https://github.com/GoogleCloudPlatform/kubernetes
After=docker.service cadvisor.service
Requires=docker.service

[Service]
EnvironmentFile=-/etc/kubernetes/config
EnvironmentFile=-/etc/kubernetes/kubelet
ExecStart=/usr/bin/kubelet \
        ${KUBE_LOGTOSTDERR} \
        ${KUBE_LOG_LEVEL} \
        ${KUBE_ETCD_SERVERS} \
        ${KUBELET_ADDRESS} \
        ${KUBELET_PORT} \
        ${KUBELET_HOSTNAME} \
        ${KUBE_ALLOW_PRIV} \
        ${KUBELET_ARGS}
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

实际上这样修改是无法直接work的，你会发现

```
ansible-playbook -i inventory setup.yml
```

还是错误，docker.socket的问题，查了半天实在无法理解，那里来的docker.socket

最后不得已，重启机器，发现再次执行没有问题了。好诡异的fedora系统！
