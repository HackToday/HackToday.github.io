title: 'Scrapy 爬取 Blog 网页内容到 MD 文件'
date: 2017-08-04 08:07:48
tags: Python/Ruby
---

传统的 Blog 网站，没有提供完善的 md 格式支持，所以书写的 Blog 文件往往不具有 export 其他格式的功能。
以我自己的 Blog 为例，这里书写的都是按照网站的 Blog 样式进行转化都是 html 页面，如果要转化对应的 md 格式，需要提取相应的标题，文本主体和其他相应的格式处理。

目标清楚了，我们首先想到的是使用爬虫工具来快速的处理的这个事情，下面 scrapy 就隆重登场了。

scrapy 作为一个开源实现的基于 python 爬虫框架，它内部的架构图具体如下：

主要包括： 爬虫引擎、调度器、下载器、具体爬虫、Item 流水线、下载器中间件和爬虫中间件
具体作用分别如下：

引擎：控制中枢，系统核心数据流的具体流动，并且相应动作发生时触发事件
调度器：接受引擎的request 输入，队列处理，再需要时提供给引擎
下载器：获取实际的web 页面，并返回给引擎，最终到具体的爬虫
爬虫：根据获取的页面，进行实际的解析处理，提取 item
Item 流水线：处理爬虫获取的 item，比如清理，验证和持久化
下载器中间件：下载器和引擎之间的中介，处理下载器返回给引擎的 response
爬虫中间件：处理爬虫的输入（response），和输出（items，requests）


下面我们一个实际的例子来说明如何使用：
创建项目非常简单，类似 django 封装的效果。

```
scrapy startproject tutorial
```

项目创建完成后，就是具体的爬虫的编写了，以我的博客首页为例： http://blog.chinaunix.net/uid/21335514/

我们需要根据上述的 URL 依次按照 年份 --》 标题 --》 内容就行爬取，存储使用简单的 json 文件格式，根据存储的 json 文件格式，就行简化的 md 格式转换。

```
class UnixBlogSpider(scrapy.Spider):
    name = 'unixblogtaker'
    start_urls = ['http://blog.chinaunix.net/uid/21335514']

    def parse(self, response):
        for year in response.css('p.Blog_p4 > span'):
            if year.css('.Blog_jia1') is None:
                continue
            blog_page = year.css('span + a::attr(href)').extract_first()
            if blog_page:
                yield response.follow(blog_page, callback=self.parse_title)

    def parse_title(self, response):
        for blog_summary in response.css('div.Blog_tit4 > b'):
            if blog_summary.css('.Blog_b1') is None:
                continue
            b_content = blog_summary.css('b + a::attr(href)').extract_first()
            if b_content:
                yield response.follow(b_content, callback=self.parse_content)
        next_page = response.css('li.next a::attr(href)').extract_first()
        if next_page is not None:
            yield response.follow(next_page, self.parse_title)

    def parse_content(self, response):
       ...... 
```

parse_content 这里处理的时候，可能需要根据一些特殊的字符含义进行相应的转化，以为后续的 md 简化转化提供便利。

运行 `scrapy crawl unixblogtaker -o blog.json` 就生成了我们的 json 文件

简单的处理上述的 content，（simplemr_tool.py 中的一部分）

```
def raw_save(content, title):
    with open(title, 'w') as new_file:
        new_file.write(re.sub('(\r\n\r\n)+', '\r\n\r\n', content).encode('utf-8'))
```

然后执行 python simplemr_tool.py blog.json 就会生成一系列的 md 博客了。


参考：
https://doc.scrapy.org/en/latest/intro/tutorial.html#creating-a-project
http://scrapy-chs.readthedocs.io/zh_CN/0.24/topics/architecture.html