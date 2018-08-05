title: 'Ruby - 字符串占位符'
date: 2014-03-03 23:08:12
tags: Python/Ruby
---

Ruby中的字符串占位符的替换值是操作字符串的常用方法，使用#进行变量或者ruby代码的求值，从而替换结果插入字符串中

```
#/usr/bin/env  ruby

book_mark = "znw123"
# following replace and insert
puts "The book mark is #{book_mark}"
#following run ruby code, string length and insert
puts "The book mark length is #{book_mark.length}"
```