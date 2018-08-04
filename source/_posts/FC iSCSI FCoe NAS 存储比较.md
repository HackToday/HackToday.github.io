title: 'FC iSCSI FCoe NAS 存储比较'
date: 2013-03-10 14:02:00
tags: 云计算
---

前言： 了解存储相关的协议，技术和区别对于理解云计算中的存储虚拟化具有重要意义，所以整理了相关的资料，给出了大概的介绍，如果要了解更多知识，可以参看后面附上的文献。

基于FC的存储
FC最初的设计是基于高性能，低延迟，无损的数据通信。一般应用到企业级对数据读写性能要求较高的环境中。

基于以太网的存储：
iSCSI和各种NAS协议， 主要利用了现有的以太网架构。


FC

```
高性能， 4GB/s 8GB/s
稳定性
硬件HBA卡支持
```

iSCSI

```
iSCSI借助IP协议来完成SCSI命令和数据的封包，发送。相比较FC，
1）增加了封包的处理流程和时间
2）受限于以太网的低速 1Gbps

iSCSI相比SCSI和SAS突破了传输距离（server到storage）的限制。

iSCSI的结构如下：

iSCSI Initiator <---> TCP connection  <----> iSCSI target
iSCSI technology can use a hardware initiator, a host bus adapter (HBA), or a
software initiator to issue requests to target devices

有软件，硬件实现

FcoE：
主要是实现通过以太网来传送FC的帧，为了实现FC的无损传送，对现有的以太网协议传送进行了改进和扩展
 聚合以太网和FC，减少机器不同adapte，线缆的数量
需要相应的软件，硬件支持
```

NAS

```
NAS协议比如 NFS，CIFS，相比iSCSI的不同在于，
iSCSI封装的是block-level的data
NFS则是基于file级别的访问，所以相对iSCSI而言增加了文件系统一级的处理，

软件实现
```

参考：
http://en.wikipedia.org/wiki/ISCSI#Software_initiator
http://www.redbooks.ibm.com/redpapers/pdfs/redp4493.pdf
http://www.lsi.com/downloads/public/sas%20switch/sas%20switch%20common%20files/channel_hostinterfacepositioningguide_pg_120310.pdf
http://www.cisco.com/en/US/netsol/ns1060/index.html
http://www.vmware.com/files/pdf/techpaper/Storage_Protocol_Comparison.pdf