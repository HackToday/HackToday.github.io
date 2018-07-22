title: OpenStack unit测试运行tox错误： ValueError: ("Expected , or end-of-list in"
date: 2015-07-17 16:50:51
tags: Python/Ruby
---


						今天pull到最新的代码，发现错误，
ValueError: ("Expected ',' or end-of-list in", "mock>=1.1;python_version!='2.6'", 'at', ";python_version!='2.6'")

后来找到https://bugs.launchpad.net/devstack/+bug/1468808

原来是对应的virtualenv版本太低导致，
解决方法就是升级virtualenv

pip install -U virtualenv                                   