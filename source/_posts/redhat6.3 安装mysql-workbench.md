title: redhat6.3 安装mysql-workbench
date: 2012-11-09 23:09:34
tags: Mysql/postgreSQL
---

redhat6.3缺少 libzip.so.1, 而这个是 mysql-workbench所需要的, 所以你需要首先安装 libzip包.方法：1. 下载rpm包http://rpm.pbone.net/index.php3/stat/4/idpl/17359193/dir/fedora_16/com/libzip-0.9.3-3.fc15.x86_64.rpm.html2. yum install libzip-0.9.3-3.fc15.x86_64.rpm3. yum install mysql-workbench-gpl-5.2.44-1el6.x86_64.rpmrefer:http://databaseblog.myname.nl/2011/03/mysql-workbench-on-rhel6.html方法：1. 下载rpm包http://rpm.pbone.net/index.php3/stat/4/idpl/17359193/dir/fedora_16/com/libzip-0.9.3-3.fc15.x86_64.rpm.html2. yum install libzip-0.9.3-3.fc15.x86_64.rpm3. yum install mysql-workbench-gpl-5.2.44-1el6.x86_64.rpmrefer:http://databaseblog.myname.nl/2011/03/mysql-workbench-on-rhel6.html