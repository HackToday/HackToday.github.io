title: 'java classloader 原理'
date: 2012-07-15 22:51:16
tags: Java
---

转自： http://blog.sina.com.cn/s/blog_6383597b0100fsiw.html

简单明了的文章，留下。

```
一．简述javaJVM和跨平台性

	这个问题方的很久的一直没有关系过，常常有人忽略的技术，但是又是相当总要的技术。作为开始我想先谈谈为什么java会这么强大。主要基于java两个特点：1）java源程序编译后产生的不是机械码，而是一种和平台太无关的字节码。然后通过各个平台的解释器解释执行。2）java采用泄后联编技术。即对域和内存的管理都是在执行的时候根据需要动态的分配。这样就是说java编译产生的字节码才是跨平台的。

二．classLoader的介绍及加载过程

	接下来我们就来说说classLoader，为什么要classLoader呢？与普通程序不同的是，Java程序（class文件）并不是本地的可执行程序。当运行Java程序时，首先运行JVM（Java虚拟机），然后再把Java
class加载到JVM里头运行，负责加载Java class的这部分就叫做Class
Loader。所以classLoader的目的在于把class文件装入到jvm中。那么classLoader又在那里的啦？又由谁调用呢？其实classLoader只是jvm的一个实现的一部分。Jvm提供的一个顶级的classLoader（bootStrap
classLoader），bootStrap
classLoader负责加载java核心的API以满足java程序最基本的需求。Jvm还提供的两个classLoader，其中Extension
ClassLoader负责加载扩展的Java
class（例如所有javax.*开头的类和存放在JRE的ext目录下的类），Application
ClassLoader负责加载应用程序自身的类。而Extension ClassLoader和Application
ClassLoader则由bootStrap classLoader加载。

三．classLoader加载的基本流程
	    
当运行一个程序的时候，JVM启动，运行bootstrap
classloader，该ClassLoader加载java核心API（ExtClassLoader和AppClassLoader也在此时被加载），然后调用ExtClassLoader加载扩展API，最后AppClassLoader加载CLASSPATH目录下定义的Class，这就是一个程序最基本的加载流程。

四．classLoader加载的方式
	 
    其实classLoader在加载class文件的时候就采用的双亲委托模式。每一个自定义ClassLoader都必须继承ClassLoader这个抽象类，而每个ClassLoader都会有一个parent
ClassLoader，我们可以看一下ClassLoader这个抽象类中有一个getParent()方法，这个方法用来返回当前ClassLoader的parent，注意，这个parent不是指的被继承的类，而是在实例化该ClassLoader时指定的一个ClassLoader，如果这个parent为null，那么就默认该ClassLoader的parent是bootstrap
classloader，这个parent有什么用呢？我们可以考虑这样一种情况，假设我们自定义了一个ClientDefClassLoader，我们使用这个自定义的ClassLoader加载java.lang.String，那么这里String是否会被这个ClassLoader加载呢？事实上java.lang.String这个类并不是
被这个ClientDefClassLoader加载，而是由bootstrap
classloader进行加载，为什么会这样？实际上这就是双亲委托模式的原因，因为在任何一个自定义ClassLoader加载一个类之前，它都会先
委托它的父亲ClassLoader进行加载，只有当父亲ClassLoader无法加载成功后，才会由自己加载，在上面这个例子里，因为
java.lang.String是属于java核心API的一个类，所以当使用ClientDefClassLoader加载它的时候，该ClassLoader会先委托它的父亲ClassLoader进行加载，上面讲过，当ClassLoader的parent为null时，ClassLoader的parent就是bootstrap
classloader，所以在ClassLoader的最顶层就是bootstrap
classloader，因此最终委托到bootstrap classloader的时候，bootstrap
classloader就会返回String的Class。

五.对于classLoader加载的一些细节说明。
	   
我们来讲解ClassLoader中的两个loadClass方法，都是用来加载class的，但是两者在作用上却有所区别。 

``` 
Class loadClass(String name)
Class loadClass(String name, boolean resolve)
```

我们看到上面两个方法声明，第二个方法的第二个参数是用于设置加载类的时候是否连接该类，true就连接，否则就不连接。说到连接，不得不在此做一下解
释，在JVM加载类的时候，需要经过三个步骤，装载、连接、初始化。装载就是找到相应的class文件，读入JVM，初始化就不用说了，最主要就说说连
接。连接分三步，第一步是验证class是否符合规格，第二步是准备，就是为类变量分配内存同时设置默认初始值，第三步就是解释，而这步就是可选的，
根据上面loadClass方法的第二个参数来判定是否需要解释，所谓的解释根据《深入JVM》这本书的定义就是根据类中的符号引用查找相应的实体，再把符号
引用替换成一个直接引用的过程。有点深奥吧，呵呵，在此就不多做解释了，想具体了解就翻翻《深入JVM吧》，呵呵，再这样一步步解释下去，那就不知道什么
时候才能解释得完了。我们再来看看那个两个参数的loadClass方法，在JAVA API 文档中，该方法的定义是protected，那也就是说该方法是被保护的，
而用户真正应该使用的方法是一个参数的那个，一个参数的loadclass方法实际上就是调用了两个参数的方法，而第二个参数默认为false，
因此在这里可以看出通过loadClass加载类实际上就是加载的时候并不对该类进行解释，因此也不会初始化该类。而Class类的forName方法则是相反，
使用forName加载的时候就会将Class进行解释和初始化，forName也有另外一个版本的方法，可以设置是否初始化以及设置ClassLoader，在此就不多讲了。

```