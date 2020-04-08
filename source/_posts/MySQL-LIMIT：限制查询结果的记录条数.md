---
title: MySQL LIMIT：限制查询结果的记录条数
tags:
  - Mysql
categories: Mysql
keywords: Mysql limit 限制查询结果的记录条数
description: Mysql limit 限制查询结果的记录条数
cover: 'https://tva2.sinaimg.cn/large/9fc55f55gy1gcz8zkfuatj20sf0iy1kx.jpg'
abbrlink: a9695717
date: 2020-03-19 14:45:49
top_img:
copyright:
---

在使用 MySQL  SELECT 语句时往往返回的是所有匹配的行，有些时候我们仅需要返回第一行或者前几行，这时候就需要用到 MySQL LIMT 子句。

基本语法如下：

```mysql
<LIMIT> [<位置偏移量>,] <行数>
```

> LIMIT 接受一个或两个数字参数。参数必须是一个整数常量。如果给定两个参数，第一个参数指定第一个返回记录行的偏移量，第二个参数指定返回记录行的最大数目。

> 第一个参数“位置偏移量”指示 MySQL 从哪一行开始显示，是一个可选参数，如果不指定“位置偏移量”，将会从表中的第一条记录开始（第一条记录的位置偏移量是 0,第二条记录的位置偏移量是 1，以此类推）；第二个参数“行数”指示返回的记录条数。

【实例1】显示`oauth_roles`表查询结果的前4行，输入的SQL语句和执行结果如下

```mysql
mysql> select * from oauth_roles limit 4;
+----+-------+
| id | roles |
+----+-------+
|  1 | users |
|  2 | admin |
|  3 | test1 |
|  4 | test2 |
+----+-------+
4 rows in set
```

由结果可以看到，该语句没有指定返回记录的“位置偏移量”参数，显示结果从第一行开始，“行数”参数为 4，因此返回的结果为表中的前 4 行记录。

若指定返回记录的开始位置，则返回结果为从“位置偏移量”参数开始的指定行数，“行数”参数指定返回的记录条数。

【实例2】在`oauth_roles`表中，使用LIMIT子句返回从第 4 条记录开始的行数为 5 的记录，输入的 SQL 语句和执行结果如下所示。

```mysql
mysql> select * from oauth_roles limit 3,5;
+----+-------+
| id | roles |
+----+-------+
|  4 | test2 |
|  5 | test3 |
|  6 | test4 |
|  7 | test5 |
|  8 | test6 |
+----+-------+
```

由结果可以看到，该语句指示 MySQL 返回从第 4 条记录行开始的之后的 5 条记录，第一个数字“3”表示从第 4 行开始（位置偏移量从 0 开始，第 4 行的位置偏移量为 3），第二个数字 5 表示返回的行数。

