title: 'ruby的warning: already initialized constant问题'
date: 2014-03-30 12:36:40
tags: Python/Ruby
---

如果使用1.8的ruby，项目编译运行经常会遇到如下的警告

```
warning: already initialized constant
```

这个就是因为load多次造成，为什么会load多次呢？ 

http://stackoverflow.com/questions/4532405/what-is-the-right-way-to-initialize-a-constant-in-ruby 给了解释。
比如下面这个测试程序（比如命名machine.rb,这个文件在~/test目录下)

```
module MyWork
  IPADDR = 0x16df
end
```

我们在另外一个主rb文件(mytestmod.rb，和machine.rb在同一目录下)需要用到上面模块中的常量

```
require './machine'
require '../test/machine'
class Book
  attr_reader :name, :price
  def initialize(name, price)
    @name = name
    @price = price
  end

end
book = Book.new('hello world', 120)
puts book.name
```

注意上面的例子，我们对同一个文件require了两次，如果使用ruby1.8,就会有

```
../test/machine.rb:4: warning: already initialized constant IPADDR
hello world
```

但是ruby1.9,不会出现这个问题

```
hello world
```