title: NTP: no server suitable for synchronization found
date: 2015-07-24 11:35:25
tags: LINUX
---


						Ntp Server 在Redhat 7配置后，发现client

 ntpdate -u <serverip>

报错： no server suitable for synchronization found

检查排错后，原来是firewall的问题，

在NTP server 配置防火墙


iptables -I INPUT -p udp --dport 123 -j ACCEPT
iptables -I OUTPUT -p udp --sport 123 -j ACCEPT

这样就可以了，注意-I, 不是-A  
确保你的CHAIN最后一条不是Reject，因为-A是在最后添加一条新的规则。                                   