title: 'ChatterBot 智能机器人玩起来'
date: 2017-07-23 18:12:10
tags: Python/Ruby
---

机器学习作为一门技术已经被应用到很多场景中，从个性化推荐到智能数据商业分析等，举不胜举，今天我们看一个简单的小例子，智能会话机器人、

所需技术： gunicorn，nginx，mongodb，mysql

ChatterBot 是一个支持多种语言的机器人，通过基于历史会话进行机器学习，从而可以产生自动应答的效果。具体原理图如下：


官方已经有一个 django 的应用例子，不过默认是使用的 sqlite db
改动： 我们使用了 mongodb，然后 django 应用对应的配置了 mysql，使用gunicorn （HTTP server）和 nginx （proxy server），

具体如下：

1. 下载 django_app

https://github.com/gunthercox/ChatterBot/tree/master/examples/django_app

2. 配置 mysql db

配置 mysql：

```
sudo apt-get update
sudo apt-get install mysql-server
sudo mysql_secure_installation
```

```
CREATE DATABASE chattraindb CHARACTER SET UTF8;

CREATE USER aiuser@localhost IDENTIFIED BY 'password';

GRANT ALL PRIVILEGES ON chattraindb .* TO aiuser@localhost;

FLUSH PRIVILEGES;
```

```
sudo apt-get install python-mysqldb

python manage.py migrate
```

3. 安装 mongdb (略，参考官方文档 4）
4. 修改 chatterbot 配置

```
  CHATTERBOT = {
      'name': 'Django ChatterBot Example',
      'trainer': 'chatterbot.trainers.ChatterBotCorpusTrainer',
      'training_data': [
         'chatterbot.corpus.english'
      ], 
     'storage_adapter': 'chatterbot.storage.MongoDatabaseAdapter',
     'database': 'train-sample',
     'django_app_name': 'django_chatterbot'
  }

```

5. 安装 gunicorn，nginx

```
sudo pip install gunicorn
sudo apt-get install nginx
```

然后进行systemd 配置

```
$ cat /etc/systemd/system/gunicorn.service
[Unit]
Description=gunicorn daemon
After=network.target

[Service]
PIDFile=/run/gunicorn/pid
User=kennan
Group=kennan
RuntimeDirectory=gunicorn
WorkingDirectory=/home/kennan/workRepo/ChatterBot/examples/django_app
ExecStart=/usr/local/bin/gunicorn  --pid /run/gunicorn/pid \
            --bind unix:/run/gunicorn/socket  example_app.wsgi
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s TERM $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

nginx 配置如下:

```
    server {
        listen          80;
        server_name     kennan-box;

        location  /static/ {
            root /home/kennan/workRepo/ChatterBot/examples/django_app/example_app;
        }   

        location / {
            proxy_pass  http://unix:/run/gunicorn/socket;
        }   
    }   
}
```

为了访问 80 端口，不会默认到 nginx 欢迎页面，需要删除默认配置

```
sudo rm -rf  /etc/nginx/sites-enabled/default
```

6. nginx 生效，就能访问了， 效果如下：