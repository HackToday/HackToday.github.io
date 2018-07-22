title: fedora 16 的fcitx配置
date: 2012-06-10 07:52:48
tags: LINUX
---


						1. 第一步安装fcitx yum install fcitx.x86_642. 设置fcitx为默认输入法alternatives --config xinputrc选择fcitx对应的序号3. 设置启动的环境变量编辑/etc/profile,在其中加入如下几行export XMODIFIERS=”@im=fcitx”export QT_IM_MODULE=fcitxexport GTK_IM_MODULE=fcitx4. 重启Fedora, 按Ctrl+回车就可以看到fcitx的输入界面了。转自：http://hzy5000.blog.163.com/blog/static/74596452012064452485/                                   