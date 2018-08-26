title: 'Django Migration 的 Cannot add foreign key constraint 问题解决'
date: 2017-06-14 07:59:20
tags: Mysql/postgreSQL
---

最近在 Django 项目执行 Migration 的时候发现一个奇怪的报错：

```
File "/usr/lib64/python2.7/site-packages/django/db/backends/mysql/base.py", line 124, in execute

return self.cursor.execute(query, args)

File "/usr/lib64/python2.7/site-packages/MySQLdb/cursors.py", line 205, in execute

self.errorhandler(self, exc, value)

File "/usr/lib64/python2.7/site-packages/MySQLdb/connections.py", line 36, in defaulterrorhandler

raise errorclass, errorvalue

django.db.utils.IntegrityError: (1215, 'Cannot add foreign key constraint')
```

在新数据库 CI Migration 测试中是没有发现这类问题的，后来 Google 发现有类似的问题和分析 [1]，于是整理一下，以备其他人参考
这是因为老系统原来的表使用的是 MyISAM 引擎，但是现在 MySQL 新版本默认的表都是 InnoDB 了，而 MyISAM 是不支持外键约束的。
MyISAM  在 MySQL 5.5.1 之前是默认的引擎
InnoDB   在 MySQL 5.5 之后是默认的引擎

所以如果你的老的表是 MyISAM，而新表对此有外键引入， 那么自然会报上面的错误了。

解决方法也很简单：（修改旧表引擎为 InnoDB），具体如下：

```
    ALTER TABLE   ENGINE = InnoDB;
```

如何查看表的信息：

1. 查看所有表的信息

```
   SHOW TABLE STATUS FROM `DB_NAME`;
```

2. 查看某个表的信息

```
    SHOW TABLE STATUS FROM `DB_NAME` LIKE 'TABLE_NAME';
```

参考资料：

1. https://stackoverflow.com/questions/6178816/django-cannot-add-or-update-a-child-row-a-foreign-key-constraint-fails
2. https://stackoverflow.com/questions/12614541/whats-the-difference-between-myisam-and-innodb
3. https://stackoverflow.com/questions/4515490/how-do-i-know-if-a-mysql-table-is-using-myisam-or-innodb-engine
4. https://dev.mysql.com/doc/refman/5.6/en/storage-engine-setting.html
5. https://github.com/divio/django-filer/issues/939

