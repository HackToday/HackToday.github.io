title: 'eclipse36，eclipse42，java的融合问题'
date: 2013-08-24 10:03:43
tags: Java
---

昨天被Junit搞的精疲力尽，使用的eclipse3.6一路出现莫名其妙的jvm运行错误，

最初怀疑java配置的问题，因为所有的代码都是在java7运行的，虽然在eclipse设置installed jre是java7，
但是系统（windows）的系统java_home没有更改，还是原来的java6。所以更改系统的java_home，发现仍然存在问题，
于是再仔细看了一下stack trace：

```
at java.lang.J9VMInternals.initialize(J9VMInternals.java:176)
at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:68)
at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
at java.lang.reflect.Constructor.newInstance(Constructor.java:528)
at org.junit.runners.BlockJUnit4ClassRunner.createTest(BlockJUnit4ClassRunner.java:187)
at org.junit.runners.BlockJUnit4ClassRunner$1.runReflectiveCall(BlockJUnit4ClassRunner.java:236)
at org.junit.internal.runners.model.ReflectiveCallable.run(ReflectiveCallable.java:15)
at org.junit.runners.BlockJUnit4ClassRunner.methodBlock(BlockJUnit4ClassRunner.java:233)
at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:68)
at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:47)
at org.junit.runners.ParentRunner$3.run(ParentRunner.java:231)
at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:60)
at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:229)
at org.junit.runners.ParentRunner.access$000(ParentRunner.java:50)
at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:222)
at org.junit.runners.ParentRunner.run(ParentRunner.java:300)
at org.eclipse.jdt.internal.junit4.runner.JUnit4TestReference.run(JUnit4TestReference.java:49)
at org.eclipse.jdt.internal.junit.runner.TestExecution.run(TestExecution.java:38)
at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.runTests(RemoteTestRunner.java:467)
at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.runTests(RemoteTestRunner.java:683)
at org.eclipse.jdt.internal.junit.runner.RemoteTestRunner.run(RemoteTestRunner.java:390)
at org.eclipse.pde.internal.junit.runtime.RemotePluginTestRunner.main(RemotePluginTestRunner.java:62)
at org.eclipse.pde.internal.junit.runtime.CoreTestApplication.run(CoreTestApplication.java:23)
at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:76)
at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
at java.lang.reflect.Method.invoke(Method.java:602)
at org.eclipse.equinox.internal.app.EclipseAppContainer.callMethodWithException(EclipseAppContainer.java:587)
at org.eclipse.equinox.internal.app.EclipseAppHandle.run(EclipseAppHandle.java:198)
at org.eclipse.core.runtime.internal.adaptor.EclipseAppLauncher.runApplication(EclipseAppLauncher.java:110)
at org.eclipse.core.runtime.internal.adaptor.EclipseAppLauncher.start(EclipseAppLauncher.java:79)
at org.eclipse.core.runtime.adaptor.EclipseStarter.run(EclipseStarter.java:353)
at org.eclipse.core.runtime.adaptor.EclipseStarter.run(EclipseStarter.java:180)
at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:76)
at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
at java.lang.reflect.Method.invoke(Method.java:602)
at org.eclipse.equinox.launcher.Main.invokeFramework(Main.java:629)
at org.eclipse.equinox.launcher.Main.basicRun(Main.java:584)
at org.eclipse.equinox.launcher.Main.run(Main.java:1438)
at org.eclipse.equinox.launcher.Main.main(Main.java:1414)

```

发现和equinox相关，检查target的Equinox是对应eclipse42的，会不会eclipse42的equinox在eclipse36跑会有问题，
于是，切换eclipse环境，用了eclipse42后，发现确实没有问题了。

虽然不敢肯定根本原因是不是这个，但是确实eclipse会提示运行不匹配的版本会有潜在问题。
之所有一直用eclipse36，是因为原来没发现类似的问题，好吧，那就迁到eclipse42吧。
