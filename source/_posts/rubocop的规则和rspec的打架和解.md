title: 'rubocop的规则和rspec的打架和解'
date: 2014-03-30 12:24:06
tags: Python/Ruby
---

rubocop是根据社区流行的ruby编码规范写的一个静态代码分析工具，
rpsec是ruby界流行的BDD测试工具

rpsec里有类似断言的关键字，expect，比如
1）判定某个变量等于123
expect(actual).to eq(123)
2）判断某个boolean值为true
expect(actual).to be true

expect还支持raise，throw错误的断言，采用block方式，如下

```
expect { dosomething }.to  raise_error
```

介绍了这么多，我们的问题是什么，rubocop里默认有个multi-line规则，就是代码里如果有block 方式 { ...}, 

```
[1, 2, 3].each { |i|
  puts i.to_s
  ...
}
```

rubocop 会提示上面的例子{.. }的warning，推荐你采用single-line 或者 do .. end的block方式， 采用do.. end 为例

```
[1, 2, 3].each do |i|
  puts i.to_s
  ...
end
```

但是，实际测试expect断言的block中，很可能长度有超过80字符的长度的时候，那么，就得采取几种方法，避过可恶的rubocop警告

方法1： 在.rubocop.yml文件禁掉Blocks，因为默认的是

```
	Blocks: 



	  Enabled: true 
```

这个方法显然是有点过了，因为它会将这个检查对整个代码都生效了，所以不是很好

方法2：使用其他block方式，绕过{}, 比如

```
expected = expect do
  ...
  ...
end
expected.to raise_error
```

或者

```
expect do
  ...
  ...
end.to raise_error
```

参考资料：

1. https://github.com/bbatsov/rubocop#configuration
2. https://github.com/bbatsov/rubocop/blob/master/config/enabled.yml
3. http://stackoverflow.com/questions/13274708/how-to-format-multiline-rspec-expect-to-change