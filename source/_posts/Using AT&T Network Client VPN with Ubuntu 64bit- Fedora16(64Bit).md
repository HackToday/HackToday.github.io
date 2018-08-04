title: 'Using AT&T Network Client VPN with Ubuntu 64bit- Fedora16(64Bit)'
date: 2012-01-06 00:23:54
tags: LINUX
---

转自： http://www.andrewferrier.com/blog/2009/01/12/using-att-network-client-vpn-with-ubuntu-64bit/

```
Ubuntu64
Install the ia32-libs package and all it’s dependencies:
sudo apt-get install ia32-libs
Install the AT&T client itself (IBM colleagues can obtain this from the OCDC website):
sudo dpkg -i --force-architecture agnclient_1.0~2.0.1.3000-3_i386.deb
          Add some symlinks:

sudo ln -s /usr/lib32/libssl.so.0.9.8 /usr/lib32/libssl.so.4
sudo ln -s /usr/lib32/libcrypto.so.0.9.8 /usr/lib32/libcrypto.so.4

还有使用这个AT&T 有时会莫名其妙的连不上了，基本都是CPU莫明其妙的100%了，只好kill掉agnclientd，然后重启，才能解决，不知道是什么原因


Fedora(64bit):
rpm -ivh agnclient-1.0-2.0.1.3003.i386
出现：
error: Failed dependencies:
libcrypto.so.4 is needed by agnclient-1.0-2.0.1.3003.i386
libcurl.so.3 is needed by agnclient-1.0-2.0.1.3003.i386
libssl.so.4 is needed by agnclient-1.0-2.0.1.3003.i386
(1) Create the necessary link for the lib
$ sudo yum install libcurl.i686   $ cd /lib $ sudo ln -s libcrypto.so.1.0.0g libcrypto.so.4
$ cd /usr/lib $ sudo ln -s libssl.so.1.0.0g libssl.so.4 $ sudo ln -s libcurl.so.4.2.0 libcurl.so.3   $ sudo ldconfig
(2)
$ sudo rpm -Uvh --nodeps agnclient-1.0-2.0.1.3002a.i386.rpm

(3)
$ sudo service agnclientd status $ sudo service agnLogd status   $ sudo service agnclientd start $ sudo service agnLogd start

Fedora 参考：
http://huang.yunsong.net/2011/att-agnclient-fedora-64.html
```
