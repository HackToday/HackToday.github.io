title: 'JVM default locale的问题'
date: 2013-04-01 21:26:25
tags: Java
---

前几天调试一个问题，搜集了一些关于JVM locale资料介绍，总结如下：

1. Windows 7

Before Java 7
对于java 7之前的JVM的default locale和操作系统format的设置相关，
具体配置在，控制面板--》地区和语言--》Formats

Java 7
java7 采用根据windows 7中的display language
具体配置在，控制面板--》地区和语言--》keyboards and language 
一般的系统如果没装Language Interface Pack，这个不会显示。具体可以点击里面的 How do I get additional display languages?
参看微软的说明

2. Linux的配置，

简单，分为永久设置 和 登录会话临时设置，

vi /etc/sysconfig/i18n 修改如下

```
LANG="zh_CN.UTF-8" 
```

临时，`export LANG=zh_CN.UTF-8`

3. 怎么确认JVM的default locale，

```
import java.util.Locale;


public class LocaleTest {
   public static void main(String[] args) {
      System.out.println(Locale.getDefault());
   }
}

```

参考资料:

http://stefanhendriks.wordpress.com/2012/02/
http://stackoverflow.com/questions/7107972/java-7-default-locale
http://stackoverflow.com/questions/10707238/locale-getdefault-returns-en-always
http://blog.ej-technologies.com/2011/12/default-locale-changes-in-java-7.html


