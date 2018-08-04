title: 'Redhat 6.3 openstack的cinder安装配置'
date: 2013-01-31 22:18:01
tags: 云计算
---

cinder 配置 安装：

1） 下载source code
2） pip install
3） yum install scsi-target-utils
4） yum install iscsi-initiator-utils.x86_64  (这两个相当于ubuntu下的 open-iscsi和tgt）
5） edit cinder config file
normally it includes :
     database config
     keystone access config
     volumes config
     message queue config
also enable cinder api in nova.conf

6) edit
/etc/lvm/lvm.conf
Add volume_list entry to /etc/lvm/lvm.conf to keep LVM from activating logical volumes created in VMs.
volume_list = ["VolGroup", "cinder-volumes" ]

volume_list should include your os related volume group.

7）add volumes dir in /etc/tgt/targets.conf


 include /var/lib/cinder/volumes/*

8) restart tgtd service

9) init cinder db
cinder-manage db sync

10) test if cinder works

10.1 create one test loop file and mount it

dd if=/dev/zero of=cinder-volumes bs=1 count=0 seek=2G

losetup /dev/loop2 cinder-volumes

10.2 init pv and create vg
pvcreate /dev/loop2

vgcreate cinder-volumes /dev/loop2


10.3 check if volume is created
pvscan

10.4 restart cinder service:
cinder api, scheduler, volume

10.5 use cinder to create volume 
cinder create --display_name test 1

10.6 verify volume created successfully 
cinder list

volume should in "available" status
