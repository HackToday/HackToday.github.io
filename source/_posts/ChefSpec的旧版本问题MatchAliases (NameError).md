title: ChefSpec的旧版本问题MatchAliases (NameError)
date: 2014-12-23 18:14:15
tags: Python/Ruby
---


						今天使用bundle exec strainer运行cookbook测试，发现报错，
问题1： 
 uninitialized constant RSpec::Matchers::BuiltIn::RaiseError::MatchAliases (NameError)

查出原来是旧版本的兼容性问题，
http://stackoverflow.com/questions/24459289/rspec-expectations-2-99-0-lib-rspec-matchers-built-in-raise-error-rb5-uninitia

修改Gemfile中的版本依赖如下：
gem 'chefspec', '~> 4.0'

问题2：
旧的Rspec测试都是基于老的Rspec2，在Rspec 3对应的已经不再支持，所以运行会出现下面的问题，
You must pass an argument rather than a block to use the provided matcher (equal true), or the matcher must implement `supports_block_expectations

具体问题参考这个帖子：
http://www.wenda.io/questions/4095574/rspec-3-vs-rspec-2-matchers.html
http://stackoverflow.com/questions/26118031/rspec-3-vs-rspec-2-matchers

具体新的Rspec格式要求参见文档如下：
http://www.rubydoc.info/github/rspec/rspec-expectations/RSpec/Matchers
https://www.relishapp.com/rspec/rspec-expectations/docs/built-in-matchers


问题3： 
RSpec 3.0 deprecates the :should way of writing specs for expecting things to happen.
也是新的版本的问题，导致旧的语法格式有warnings，
Fix 方法1）
这个帖子介绍的很详细，包含fix方法，
http://makandracards.com/makandra/25409-how-to-remove-rspec-old-syntax-deprecation-warnings

Inside spec/spec_helpber.rb, set rspec-expectations’ and/or rspec-mocks’ syntax as following:


RSpec.configure do |config|
  # ...
  config.mock_with :rspec do |c|
    c.syntax = [:should, :expect]
  end
  config.expect_with :rspec do |c|
    c.syntax = [:should, :expect]
  end
end

官方说明：
https://www.relishapp.com/rspec/rspec-expectations/docs/syntax-configuration

Fix 方法2）
还有一种fix方式是修改，转换旧的格式语法，有一个资料介绍一个自动转换工具：
http://yujinakayama.me/transpec/
暂时没有使用上面的方法，有空的话，可以尝试一下。

                                   