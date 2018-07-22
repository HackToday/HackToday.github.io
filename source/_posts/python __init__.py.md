title: 'python __init__.py'
date: 2011-12-04 17:48:40
tags: Python/Ruby
---


最近接触了python，看到__init__.py 感到很奇怪。经过查资料发现，这是python引进的package管理机制的产物。 cnblog有篇文章写的比较清晰，转载与此，
仅作个人资料查看。

http://www.cnblogs.com/tqsummer/archive/2011/01/24/1943273.html

```
python中的Module是比较重要的概念。常见的情况是，事先写好一个.py文 件，在另一个文件中需要import时，将事先写好的.py文件拷贝 到当前目录，或者是在sys.path中增加事先写好的.py文件所在的目录，然后import。这样的做法，对于少数文件是可行的，但如果程序数目很 多，层级很复杂，就很吃力了。
有没有办法，像Java的Package一样，将多个.py文件组织起来，以便在外部统一调用，和在内部互相调用呢？答案是有的。
主要是用到python的包的概念，python __init__.py在包里起一个比较重要的作用
要弄明白这个问题，首先要知道，python在执行import语句时，到底进行了什么操作，按照python的文档，它执行了如下操作：
第1步，创建一个新的，空的module对象（它可能包含多个module）；
第2步，把这个module对象插入sys.module中
第3步，装载module的代码（如果需要，首先必须编译）
第4步，执行新的module中对应的代码。

在执行第3步时，首先要找到module程序所在的位置，其原理为：
如
 果需要导入的module的名字是m1，则解释器必须找到m1.py，它首先在当前目录查找，然后是在环境变量PYTHONPATH中查找。 
PYTHONPATH可以视为系统的PATH变量一类的东西，其中包含若干个目录。如果PYTHONPATH没有设定，或者找不到m1.py，则继续搜索
 与python的安装设置相关的默认路径，在Unix下，通常是/usr/local/lib/python。
事
实上，搜索的顺序是：当前路径 
（以及从当前目录指定的sys.path），然后是PYTHONPATH，然后是python的安装设置相关的默认路径。正因为存在这样的顺序，如果当前
路径或PYTHONPATH中存在与标准module同样的module，则会覆盖标准module。也就是说，如果当前目录下存在xml.py，那么执
 行import xml时，导入的是当前目录下的module，而不是系统标准的xml。

了解了这些，我们就可以先构建一个package，以普通module的方式导入，就可以直接访问此package中的各个module了。

Python中的package定义很简单，其层次结构与程序所在目录的层次结构相同，这一点与Java类似，唯一不同的地方在于，python中的package必须包含一个__init__.py的文件。
例如，我们可以这样组织一个package:

package1/
    __init__.py
    subPack1/
        __init__.py
        module_11.py
        module_12.py
        module_13.py
    subPack2/
        __init__.py
        module_21.py
        module_22.py
    ……

__init__.py可以为空，只要它存在，就表明此目录应被作为一个package处理。当然，__init__.py中也可以设置相应的内容，下文详细介绍。

好了，现在我们在module_11.py中定义一个函数：

def funA():
    print "funcA in module_11"
    return

在顶层目录（也就是package1所在的目录，当然也参考上面的介绍，将package1放在解释器能够搜索到的地方）运行python:

>>>from package1.subPack1.module_11 import funcA
>>>funcA()
funcA in module_11

这样，我们就按照package的层次关系，正确调用了module_11中的函数。

细心的用户会发现，有时在import语句中会出现通配符*，导入某个module中的所有元素，这是怎么实现的呢？
答案就在__init__.py中。我们在subPack1的__init__.py文件中写

__all__ = ['module_13', 'module_12']

然后进入python

>>>from package1.subPack1 import *
>>>module_11.funcA()
Traceback (most recent call last):
  File "", line 1, in 
ImportError: No module named module_11

也就是说，以*导入时，package内的module是受__init__.py限制的。

好了，最后来看看，如何在package内部互相调用。
如果希望调用同一个package中的module，则直接import即可。也就是说，在module_12.py中，可以直接使用

import module_11

如果不在同一个package中，例如我们希望在module_21.py中调用module_11.py中的FuncA，则应该这样：

from module_11包名.module_11 import funcA
```