---
title: MySQL删除数据表
tags:
  - Mysql
categories: Mysql
keywords: Mysql删除数据表 DROP TABLE
description: Mysql删除数据表 DROP TABLE
cover: 'https://tva2.sinaimg.cn/large/9fc55f55gy1gclj2vba2bj20dv08owen.jpg'
abbrlink: b967e26d
date: 2020-03-07 17:26:28
top_img:
copyright:
---

在MySQL数据库中，对于不在需要的数据表，可以将其从数据库中删除。

在删除表的同时，表的结构和表的所有的数据都会被删除，因此在删除表之前最好先备份，以避免造成无法换回的损失。

下面我们来了解一下 MySQL 数据库中数据表的删除方法。

# 基本语法

使用 ```DROP TABLE``` 语句可以删除一个或多个数据表，语法格式如下：

```mysql
DROP TABLE [IF EXISTS] 表名1 [ ,表名2, 表名3 ...]
```

> 两点注意： 
>
> -  用户必须拥有执行 DROP TABLE 命令的权限，否则数据表不会被删除。
> -  表被删除时，用户在该表上的权限不会自动删除。

#  删除表的实例

## 【实例1】

选择数据库 test，创建tb_test 数据表，输入的 SQL 语句和运行结果如下所示。

```mysql
mysql> create table tb_test
    -> (
    -> id int(11) primary key auto_increment,
    -> name varchar(50)
    -> );
Query OK, 0 rows affected

mysql> show tables;
+----------------+
| Tables_in_test |
+----------------+
| student2       |
| tb_test        |
+----------------+
2 rows in set

mysql> 
```

由运行结果可以看出，test 数据库中有 tb_test 和 student2 两张数据表。

删除数据表 tb_test ，输入的 SQL 语句和运行结果如下所示：

```mysql
mysql> drop table tb_test;
Query OK, 0 rows affected

mysql> show tables;
+----------------+
| Tables_in_test |
+----------------+
| student2       |
+----------------+
1 row in set

mysql> 
```

## 【实例2】

当删除一个不存在的数据库会报错，如下：

```mysql
mysql> drop table tb_test;
1051 - Unknown table 'test.tb_test'
mysql> 
```

这时，可以使用如下的SQL语句执行

```mysql
mysql> drop table if exists tb_test;
Query OK, 0 rows affected

mysql> 
```

在`drop table `后面加上`if exists`之后，意思就是如何判断是否存在这个数据表，如何有，就执行删除。