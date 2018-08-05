title: 'ruby和python的ini纠纷'
date: 2014-03-09 23:19:29
tags: Python/Ruby
---

ruby有两个gem在ini文件的解析处理上用的比较多，一个是inifile，还有另外一个扩展的iniparse

根据 [1]介绍，iniparse相比inifile主要有三个优点
（1） 支持重复option项，这个在一些开源项目的ini文件可以看到
（2）ini 文件写的时候保留空格和缩进行
（3）保持对section和option的顺序

如果你不需要这些功能，iniparse建议你还是inifile吧。

其实 iniparse和inifile在对ini文件的解析上的不同还在约定上有点区别， 
（1） = 的区别
比如下面的一个section

```
[MYWORK]
test = nice=3
```

上面的这个形式，iniparse是支持的，会认为 option是test对应的value是， nice=3,
但是inifile就会报错，不支持上面的解析格式

为什么会提到这种写法呢， 这时因为有些ini文件配置的数据库地址
connection = mysql://*****?charset=utf-8

显然inifile爱莫能助，因为它的解析中，
```
...

elsif scanner.scan(%r/#{@param}/)
    if property.empty?
        property = string.strip
        string.slice!(0, string.length)
    else
        parse_error
    end
....

```

第一次， property赋值connection，但是往后解析再次遇到=，他会认为，有两个property存在，所以就parse_error了

（2）对于奇葩的ini混合式写法， ruby的ini解析都有问题
比如，有些python的ini文件，这样写

```
[composite:metadata]
use = egg:Paste#urlmap
/: meta
```

采用的是混合式写法，:和=都可以作为分隔符

inifile和iniparse搞不定这个（也许我没找到正确的调用方式，不过inifile的源码好像不支持）
pyhton的ConfigParser可以搞定上面的混合式写法的解析处理

（3）inifile的调用的可选参数

```
 def initialize( content = nil, opts = {} )
    opts, content = content, nil if Hash === content
    @content = content
    @comment = opts.fetch(:comment, ';#')
    @param = opts.fetch(:parameter, '=')
    @encoding = opts.fetch(:encoding, nil)
    @escape = opts.fetch(:escape, true)
    @default = opts.fetch(:default, 'global')
    @filename = opts.fetch(:filename, nil)
```

可以看到，默认的comment的是;=两种，所以一些ini文件
比如

```
[composite:metadata]
use = egg:Paste#urlmap
```
如果你不传入 {:comment => ';'}, 就会将#urlmap作为注释了
综上，可见ruby和python的ini解析还有有不少坑的

参考资料:

1. http://www.ruby-doc.org/gems/docs/i/iniparse-1.1.5/README_rdoc.html
2. http://docs.python.org/2.7/library/configparser.html

	
