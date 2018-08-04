title: 'OSGI的tracking service使用'
date: 2012-12-16 12:31:21
tags: Java
---

在eclipse的OSGI框架中，tracking service是OSGI很重要的一个功能，通过对服务的查询来动态的获取相应服务，例子：

我们建立两个bundle，一个是提供sayHello的服务，另外一个bundle来使用sayHello服务

```
bundle 1： com.javaworld.sample.service

结构如下：

├── META-INF
│   └── MANIFEST.MF
└── src
    └── com
        └── javaworld
            └── sample
                └── service
                    ├── HelloService.java
                    └── impl
                        ├── HelloServiceActivator.java
                        ├── HelloServiceFactory.java
                        └── HelloServiceImpl.java

```

```
bundle 2： com.javaworld.sample.helloworld

├── build.properties
├── META-INF
│   └── MANIFEST.MF
└── src
    └── com
        └── javaworld
            └── sample
                └── helloworld
                    ├── Activator.java
                    └── HelloServiceTracker.java

HelloServiceTracker 继承 osgi的ServiceTracker， 通过构造函数中说明需要track哪些服务，

  public HelloServiceTracker(BundleContext context) {
        super(context, HelloService.class.getName(), null);
   }
  
  public Object addingService(ServiceReference reference) {
        System.out.println("Inside HelloServiceTracker.addingService " +
                                        reference.getBundle());
        return super.addingService(reference);
    }
   
  public void removedService(ServiceReference reference, Object service) {
        System.out.println("Inside HelloServiceTracker.removedService " +
                                          reference.getBundle());
        super.removedService(reference, service);
    }  
```

在helloservice中，HelloServiceActivator的bundle启动过程中会注册服务，
helloServiceRegistration = context.registerService(HelloService.class.getName(), 
                                                   helloServiceFactory, null);


helloworld的bundle就可以通过service tracker的 getService来返回相应的service对象

```
    /*
     * (non-Javadoc)
     * @see org.osgi.framework.BundleActivator#start(org.osgi.framework.BundleContext)
     */
    public void start(BundleContext context) throws Exception {
        System.out.println("Hello World!!");
        helloServiceTracker= new HelloServiceTracker(context);
        helloServiceTracker.open();
        HelloService helloService = (HelloService)helloServiceTracker.getService();
        System.out.println(helloService.sayHello());
    }
    
    /*
     * (non-Javadoc)
     * @see org.osgi.framework.BundleActivator#stop(org.osgi.framework.BundleContext)
     */
    public void stop(BundleContext context) throws Exception {
        System.out.println("Goodbye World!!");
        helloServiceTracker.close();
    }

```

上次写了忘记给参考资料，实在抱歉。

参考资料：

1. http://www.javaworld.com/javaworld/jw-03-2008/jw-03-osgi1.html?page=5
                                   