title: 'Maven的POM介绍'
date: 2012-12-15 19:11:35
tags: Java
---

POM： Project Object Model  是maven项目架构里重要的基本单元

最小的POM需具备以下的元素：

- project root
- modelVersion - should be set to 4.0.0
- groupId - the id of the project's group.
- artifactId - the id of the artifact (project)
- version - the version of the artifact under the specified group

例如：

```
  4.0.0 
  com.mycompany.app
  my-app 
  1
```

POM 使用 `::` 来唯一标记一个项目，它们是maven的 
因为软件经常涉及到很多projects，那么如何来维护projects之间的关系非常重要，POM中有两个非常重要的特性：
project inheritance和project aggregation，两者主要是看待projects之间的关系的角度不同，其实是殊途同归。

假设前提，我们有两个projects，com.mycompany.app:my-module:1 com.mycompany.app:my-app:1
其中 com.mycompany.app:my-app:1 是com.mycompany.app:my-module:1 的 parent artifact

(1) 组织形式如下的时候

```
 . |-- my-module 
   |    `-- pom.xml 
   `-- pom.xml 
```

首先看看project inheritance： my-module的pom需要加入parent的元素，com.mycompany.app:my-module:1's POM 如下


```
 com.mycompany.app 
 my-app 
 1 

 4.0.0
 com.mycompany.app
 my-module 
1 
```

对于project aggregation：通过在my-app中指定相应的子module，从而使得 parent project 知道所有的 modules 里面
很重要的一点是在packaging里指出为pom 


```
  4.0.0
  com.mycompany.app 
  my-app
  1 
  pom 

   my-module 

```

(2) 组织形式如下的时候：


```
. |-- my-module 
  |    `-- pom.xml 
  `-- parent 
      `-- pom.xml
```

首先看看project inheritance：
通过relativepath来配合设置相应的目录结构 


```  
   com.mycompany.app 
   my-app 
   1 
   .../parent/pom.xml
  
  
   4.0.0
   my-module
```

对于project aggregation：

```
   4.0.0 
   com.mycompany.app 
   my-app 
   1 
   pom
   
     ../my-module 
```   

(3) 你也可以将inheritance和aggregation联合起来，应用下面三条： 



Specify in every child POM who their parent POM is.
Change the parent POMs packaging to the value "pom" .
Specify in the parent POM the directories of its modules (children POMs) 

对于如下的形式： 

```
. |-- my-module 
  |    `-- pom.xml 
  `-- parent 
      `-- pom.xml 

```

com.mycompany.app:my-app:1's POM

```
  4.0.0 
  com.mycompany.app 
  my-app 
  1 
  pom 
  
  ../my-module 
```  

com.mycompany.app:my-module:1's POM

```
     com.mycompany.app 
     my-app 
     1 
     ../parent/pom.xml 
  
    4.0.0 
    my-module 
```

参考资料：

1. http://maven.apache.org/guides/introduction/introduction-to-the-pom.html
2. http://www.oracle.com/technetwork/cn/community/java/apache-maven-getting-started-1-406235-zhs.html
