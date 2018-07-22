title: ovf学习1--虚拟化的映像的开放标准格式
date: 2011-12-18 15:35:39
tags: 虚拟化
---


						Open Virtualization Format(OVF)是针对虚拟机快速发布，部署的一项业界标准协议，它以一种开放，可移植，安全的特点被越来越多的云计算厂商所支持。OVF协议中最重要的一个概念就是OVF Package，使用它就可以快速的完成虚拟机的快速安装和配置，它包括以下部分：1) ovf 描述符文件, 后缀名是 .ovf。 一个XML格式文档，关于package的metadata和内容。描述的是虚拟化硬件的配置需求，产品细节，和许可证等2) ovf清单文件，后缀名是.mf， 主要是关于package内文件的SHA-1摘要3) ovf证书，后缀是.cert，通过对.mf文件的数字化签名来确保package有效性4) disk映像文件，这个就是核心的安装了操作系统，应用软件的磁盘映像，有多种厂商的格式5) 其他的一些资源文件，比如iso镜像文件    它主要包含的是关于虚拟机的软件化配置， ovf-env.xml是一种xml格式文件，一般位于ISO映像的根目录 OVF package可以以单个文件存在，后缀名是.ova     许多厂商的虚拟化管理软件都支持OVF，比如IBM的System Director, Vmware的Vsphere，通过专门的映像制作工具，可以制作相应的OVF packge，这样就可以方便的到不同的环境中进行导入和导出。参考信息：1. 弯曲评论，  ovf协议：虚拟机的mp3格式2. DMTF， OVF 规范                                   