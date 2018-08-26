title: 'python 相同的 url 下载行为不一致调查 （Tornado）'
date: 2017-08-24 08:23:58
tags: Web开发
---

前几天遇到一个神奇的问题，同样的一个 url，在浏览器访问这些远端的机器时，会出现不同的行为：直接下载或是打开。为什么行为不一致呢？ 很奇怪。

没有很好的办法，只能从 debug 浏览器的 request 请求开始，发现一台机器是 application/octet-strea，另外一台是 text/plain
同样的 url 在不同的机器上有这个奇怪的表现，不多说了，肯定是 tornado web 中的一些代码逻辑导致的，于是决定翻源码。
https://github.com/python/cpython/blob/master/Lib/mimetypes.py
https://github.com/tornadoweb/tornado/blob/branch4.3/tornado/web.py

定位到决定 content 内容的源码如下：
点击(此处)折叠或打开

```
def get_content_type(self):

    """Returns the ``Content-Type`` header to be used for this request.



    .. versionadded:: 3.1

    """

    mime_type, encoding = mimetypes.guess_type(self.absolute_path)

    # per RFC 6713, use the appropriate type for a gzip compressed file
```

mimetypes.py 中会根据不同的文件扩展类型来推测 mime_type ，一台机器是 application/octet-strea，另外一台是 text/plain
这就奇怪了，同样的操作系统和 tornadoweb 版本

再细看 mimetypes.py 源码发现，里面有一些蹊跷

```
knownfiles = [

"/etc/mime.types",

"/etc/httpd/mime.types", # Mac OS X

"/etc/httpd/conf/mime.types", # Apache

"/etc/apache/mime.types", # Apache 1

"/etc/apache2/mime.types", # Apache 2

"/usr/local/etc/httpd/conf/mime.types",

"/usr/local/lib/netscape/mime.types",

"/usr/local/etc/httpd/conf/mime.types", # Apache 1.2

"/usr/local/etc/mime.types", # Apache 1.3

]
```

其中 /etc/mime.types 一台机器上没有，另外一台包含， 这个文件中包含了 .log 结尾的文件情况，那么看看是哪个包导致的不同：

```
# rpm -q --whatprovides /etc/mime.types
mailcap-xxx-.el7.noarch
```

后来确认才知道，一台机器当时使用的最小化安装，所以遗漏了一些后期的标准化安装的包。这个差别才导致了行为的差异。
