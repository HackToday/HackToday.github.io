title: '使用Docker进行天气预报'
date: 2016-03-15 13:08:44
tags: 云计算
---

Docker的容器化带来的一个好处是可以方便的尝试各种app的快速部署和体验，github上有一个有趣的天气预报app，https://github.com/schachmat/wego

步骤如下：
1. 部署一个支持golang运行环境的容器
docker run --rm  -it golang bash


2. 安装wego
运行wego，第一次安装的时候会生成一个配置文件，~/.wegorc
其中需要配置一个key，这个key是可以支持调用worldweatheronline.com网站的API，
你需要到对应的网站免费注册，获得相应的免费API key，配置到上面的这个文件中。


3.  配置城市
city=Beijing

4. 再次运行wego， 就获得了结果
