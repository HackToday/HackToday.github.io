title: 'DevOps, SRE and system engineering 的一些必备知识技能'
date: 2017-02-05 19:41:20
tags: 系统运维
---

有一篇很不错的英文如下，总结了一些基本的知识点，这个对于工作和面试的一些方面有参考价值，希望对其他人有帮助之处：

来源： https://hackernoon.com/the-must-know-checklist-for-devops-system-reliability-engineers-f74c1cbf259d?imm_mid=0ec6e8#.1qvl6v6uoSource


我本着学习和兴趣的需要，总结分类如下：

系统基础

熟悉理解和使用 *nix 系统，了解不同发行版的区别，理解系统内部的基本原理
熟练掌握查看系统、CPU 内存等性能数据
理解和使用 cron，
理解和掌握一种shell，shell相关文件的配置。知道 bash/dash/sh/ash/zsh 区别
设置环境变量，如何持久化
知道 init 系统原理，upstart systemd，会使用systemctl journalctl 分析和调试问题
会编译源代码
理解ext4, ntfs, fat 等文件系统，知道Union文件系统
系统如何备份，备份策略，如何验证工作
md5, SHA 验证和使用

网络

配置和理解 IP/DNS/DHCP 的原理，router 查看和调试问题
SSH keys 和无密码访问配置
会配置firewal 设置rule，
知道TCP/UDP 区别， OSI模型和 TCP/IP 模型， 知道Vxlan
熟悉tcpdump wireshark等分析调试网络问题
熟悉使用进程管理ps, top, htop, atop，性能查看nmon, iostat, sar, vmstat
网络分析nmap, tcpdump, ping, traceroute, airmon, airodump等
熟悉完全协议相关的TLS, STARTTLS, SSL, HTTPS, SCP, SSH, SFTP, FTPS
知道 PPTP, OpenVPN, L2TP/IPSec 不同
SSL/TLS 如何工作，数字证书（https）如何工作
HTTP不同状态码的意义

Web

懂得配置 Web Server（Nginx Apache）
Nginx、Apache 区别
配置反向代理（Nginx..） 负载均衡，缓存服务器

Devops

懂得配置管理工具，例如chef puppet ansible etc
懂得Jenkfins 等 CI的基本配置和使用
监控系统的Nagios, Zabix, Sensu, prometheus 搭建和配置使用
了解chatops，基本的一些流行的框架

开发基础

脚本语言的使用python ruby 等
分布式版本控制工具 Git 的使用和基本原理
Vim Vagrant 开发环境的使用，熟悉一种 IDE 环境
熟悉理解 docker 和容器的底层技术
熟悉理解 Kubernetes/Mesos/SwarmKit 等相关的技术和区别
深入掌握 DB （mysql 等）
熟悉Redis/Memcache 类似的工具

架构

理解 PaaS/Iaas/Saas/CaaS/FaaS/DaaS and serverless 架构关联和区别
理解monolithic和microservices不同，和优缺点
理解stateful 和 stateless应用
如何设计可扩展，高可靠的分布式系统
熟悉理解微服务相关的API和服务知识，例如 RESTfull, RESTful-like, API gateways, Lambda functions, serverless computing,SOA, SOAP, JMS, CRUD

学习方法

Google, StackOverflow, Quora 和其他论坛的使用
跟踪其他知名公司 blog， github 开源项目
