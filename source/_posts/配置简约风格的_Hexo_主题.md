title: '配置简约风格的 Hexo 主题'
date: 2018-11-18 22:00:00
tags: Linux
---

今天写博客的时候，突然感觉这个 landscape 主题不优雅，想换一个简约优雅的主题风格。
查找了一下，发现 Hexo 的 `Next` 风格很协调，所以打算切换一下看看效果。

<!-- more -->

动手操作还是比较简单，具体如下：

### 安装 Next 稳定版本

```
$ mkdir themes/next
$ curl -s https://api.github.com/repos/iissnan/hexo-theme-next/releases/latest | grep tarball_url | cut -d '"' -f 4 | wget -i - -O- | tar -zx -C themes/next --strip-components=1
```

### 采用 Hexo Data 文件完成主题配置

这种方式是为了解决 Next Git 更新主题冲突的问题，每次冲突需要用户去解决，很是不方便。

Next 推荐采用唯一配置文件，具体位置放在 `source/_data/next.yml` 下面。

```
$ mkdir -p source/_data
$ cd source/_data

# 将 site 相关的配置和 theme 主题相关的配置都拷贝到 next.yml 
```

### 生成网站内容，并且部署

```
$ hexo g  --config source/_data/next.yml
$ hexo deploy  --config source/_data/next.yml
```

*注明* 

- 可以使用 `hexo server --debug --config source/_data/next.yml` 在本地查看效果
- 切换主题的时候最也执行 hexo clean 来清除缓存

### 关于更多的配置

- Tags 配置

```
$ hexo new page tags

$ cat  source/tags/index.md
title: tags
date: 2018-11-18 21:36:22
type: tags
---

```

- Social 配置 （连接到 Github， 打开即可）

- 访问站点统计 （busuanzi_count)

  参考 http://ibruce.info/2015/04/04/busuanzi/

- Scheme 切换（Gemini 感觉这个左右分栏的布局更上眼）


### 参考资料

http://theme-next.iissnan.com/getting-started.html
https://github.com/iissnan/hexo-theme-next
https://hexo.io/zh-cn/docs/themes.html
