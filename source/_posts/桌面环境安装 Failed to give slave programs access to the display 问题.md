title: 桌面环境安装 Failed to give slave programs access to the display 问题
date: 2017-01-11 18:42:52
tags: LINUX
---


						在测试环境安装桌面环境发现 gdm 有错误如下：

Failed to give slave programs access to the display

尝试安装 gnome 3 桌面环境 gdm 启动不再出现类似的错误。


yum group install "GNOME Desktop"

但是 gnome desktop 和 gdm 不是一个东西，gdm 只是一个 diplay manager

gnome desktop 使用也不完全依赖 gdm                                   