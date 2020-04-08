---
title: 'MySQL修改数据库:ALTER DATABASE用法简介'
tags:
  - Mysql
categories: Mysql
keywords: ALTER DATABASE
description: 修改数据库 ALTER
cover: 'http://tva4.sinaimg.cn/large/9fc55f55gy1gce746x6iqj20qf0o4god.jpg'
abbrlink: b1cf4eaf
date: 2020-03-01 09:45:00
top_img:
copyright:
---

在 MySQL 数据库中只能对数据库使用的字符集和校对规则进行修改，数据库的这些特性都储存在 db.opt 文件中。下面我们来介绍一下修改数据库的基本操作。

##### ALTE DATABASE的基本语法

在 MySQL 中，可以使用 ```ALTER DATABASE``` 来修改已经被创建或者存在的数据库的相关参数。修改数据库的语法格式为：

```MYSQL
ALTER DATABASE [数据库名] { 
[ DEFAULT ] CHARACTER SET <字符集名> |
[ DEFAULT ] COLLATE <校对规则名>}
```

> 语法说明如下：
>
> 1、ALTER DATABASE 用于更改数据库的全局特性。
>
> 2、使用 ALTER DATABASE 需要获得数据库 ALTER 权限。
>
> 3、数据库名称可以忽略，此时语句对应于默认数据库。
>
> 4、CHARACTER SET 子句用于更改默认的数据库字符集。

##### 案例详解

##### 实例1：指定字符集修改为 gb2312

查看 test 数据库的定义声明的执行结果如下所示：

```MYSQL
mysql> show create database test;
+----------+-----------------------------------------------------------------+
| Database | Create Database                                                 |
+----------+-----------------------------------------------------------------+
| test     | CREATE DATABASE `test` /*!40100 DEFAULT CHARACTER SET gb2312 */ |
+----------+-----------------------------------------------------------------+
1 row in set

mysql> 
```

 使用命令行工具将数据库 test 的指定字符集修改为 gb2312，默认校对规则修改为 utf8_unicode_ci，输入 SQL 语句与执行结果如下所示：

```mysql
mysql> alter database test
    -> default character set gb2312
    -> default collate gb2312_chinese_ci;
Query OK, 1 row affected

mysql> show create database test;
+----------+-----------------------------------------------------------------+
| Database | Create Database                                                 |
+----------+-----------------------------------------------------------------+
| test     | CREATE DATABASE `test` /*!40100 DEFAULT CHARACTER SET gb2312 */ |
+----------+-----------------------------------------------------------------+
1 row in set

mysql> 
```

