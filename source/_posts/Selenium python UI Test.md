title: Selenium python UI Test
date: 2016-08-14 10:18:41
tags: Python/Ruby
---


	
	
		python中如何容易的使用selenium来进行APP的测试，可以从如下考虑：

1.Python中有个方便的库进行快速的集成和测试，py.test，pytest支持一系列的插件，其中就包含了 对于selenium的支持，使用方式如下：

def test_example(selenium):
    selenium.get('http://www.example.com')

py.test --driver Firefox

2. Selenium广泛使用的远程测试是使用Remote Driver，具体如下：
    py.test --driver Remote --host selenium.hostname --port 5555 --capability browserName firefox

一般测试使用例子如下（远程测试）：
py.test --base-url http://10.221.83.203:8081 --driver Remote --host 10.15.240.120 --capability browserName firefox

3.如果采用了marker方式，可以指定运行某类测试：

py.test --base-url http://10.221.83.203:8081 --driver Remote --host 10.15.240.120 --capability browserName firefox  -m negative

这里就只会运行 pytest.mark.negative 类型的测试
	

	
	
		python中如何容易的使用selenium来进行APP的测试，可以从如下考虑：

1.Python中有个方便的库进行快速的集成和测试，py.test，pytest支持一系列的插件，其中就包含了 对于selenium的支持，使用方式如下：

def test_example(selenium):
    selenium.get('http://www.example.com')

py.test --driver Firefox

2. Selenium广泛使用的远程测试是使用Remote Driver，具体如下：
    py.test --driver Remote --host selenium.hostname --port 5555 --capability browserName firefox

一般测试使用例子如下（远程测试）：
py.test --base-url http://10.221.83.203:8081 --driver Remote --host 10.15.240.120 --capability browserName firefox

3.如果采用了marker方式，可以指定运行某类测试：

py.test --base-url http://10.221.83.203:8081 --driver Remote --host 10.15.240.120 --capability browserName firefox  -m negative

这里就只会运行 pytest.mark.negative 类型的测试
	

		python中如何容易的使用selenium来进行APP的测试，可以从如下考虑：

1.Python中有个方便的库进行快速的集成和测试，py.test，pytest支持一系列的插件，其中就包含了 对于selenium的支持，使用方式如下：

def test_example(selenium):
    selenium.get('http://www.example.com')

py.test --driver Firefox

2. Selenium广泛使用的远程测试是使用Remote Driver，具体如下：
    py.test --driver Remote --host selenium.hostname --port 5555 --capability browserName firefox

一般测试使用例子如下（远程测试）：
py.test --base-url http://10.221.83.203:8081 --driver Remote --host 10.15.240.120 --capability browserName firefox

3.如果采用了marker方式，可以指定运行某类测试：

py.test --base-url http://10.221.83.203:8081 --driver Remote --host 10.15.240.120 --capability browserName firefox  -m negative

这里就只会运行 pytest.mark.negative 类型的测试
	