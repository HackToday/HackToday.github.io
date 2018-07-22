title: UTC的Datetime和ISO处理
date: 2016-08-14 10:09:46
tags: Python/Ruby
---


						问题1， Django中集成了相应的函数来处理这种转化如下

参考 http://www.dannysite.com/blog/122/

>>> from django.utils.timezone import utc
>>> from django.utils.timezone import localtime
>>> now = datetime.datetime.utcnow().replace(tzinfo=utc)
>>> localtime(now)
datetime.datetime(2013, 12, 5, 0, 3, 13, 122000, tzinfo= CST+8:00:00 STD>) 
  
问题2， 还有一种情况，对应的是ISO8601 字符串，要转化成对应的datetime对象 
一种是用iso8601包，但是这种情况下需要单独通过pip安装iso8601， 
参考：https://julien.danjou.info/blog/2015/python-and-timezones 
>>> import iso8601
>>> iso8601.parse_date(utcnow().isoformat())

还有一种方式是用传统的python的dateutil包：
参考：http://stackoverflow.com/questions/969285/how-do-i-translate-a-iso-8601-datetime-string-into-a-python-datetime-object 
 
import dateutil.parser
yourdate = dateutil.parser.parse(datestring)

如果本身不是字符串，而是对应的datetime对象，可以直接使用 https://docs.djangoproject.com/en/1.9/topics/i18n/timezones/ 
localtime来处理

 
问题3， 如果需要特殊的处理，可以写custom的 template filter，具体如下：
https://docs.djangoproject.com/en/1.8/howto/custom-template-tags/ 

这种方式可以处理普通的template中无法处理的情况
                                   