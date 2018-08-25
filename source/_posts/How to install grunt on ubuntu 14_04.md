title: 'How to install grunt on ubuntu 14.04?'
date: 2015-04-03 14:29:31
tags: JavaScript
---

今天要调试运行一个程序，需要grunt，发现运行grunt提示：

```
    /usr/bin/env: node: No such file or directory
```

研究+bing后发现，

grunt 需要依靠npm安装，但是npm需要nodejs版本和ubuntu默认源的版本不匹配，
所以需要按照下面的步骤来安装

```
http://www.tuicool.com/articles/YNJfAjU 
```

简化说来，就是更新确保nodejs的版本合符grunt的要求。
