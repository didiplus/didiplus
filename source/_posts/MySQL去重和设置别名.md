---
title: MySQL去重和设置别名
tags:
  - Mysql
categories: Mysql
keywords: Mysql如何去重、Mysql设置别名
description: Mysql如何去重、Mysql设置别名
cover: 'https://tvax1.sinaimg.cn/large/9fc55f55ly1gcx8zprq64j20sg0lctlb.jpg'
abbrlink: 259e736e
date: 2020-03-17 20:51:05
top_img:
copyright:
---

# 去重

在使用MySQL SELECT 语句查询的数据返回的是索引匹配的行。

【实例1】，查询 `visits`表中所有 `ip_counts`的执行结果如下所示。

```mysql
mysql> select ip_counts from visits;
+-----------+
| ip_counts |
+-----------+
|         1 |
|         2 |
|         2 |
+-----------+
3 rows in set
```

可以看到查询结果返回了 3条记录，其中有一些重复的 age 值，有时出于对数据分析的要求，需要消除重复的记录值。这时候就需要用到 DISTINCT 关键字指示 MySQL 消除重复的记录值，语法格式为：

```mysql
SELECT DISTINCT <字段名> FROM <表名>;
```

【实例2】查询 `visits`表中 `ip_counts`字段的值，返回 `ip_counts`字段的值且不得重复，输入的 SQL 语句和执行结果如下所示。

```mysql
mysql> select distinct ip_counts from visits;
+-----------+
| ip_counts |
+-----------+
|         1 |
|         2 |
+-----------+
2 rows in set
```

#  MySQL AS 设置别名

在使用 MySQL 查询时，当表名很长或者执行一些特殊查询的时候，为了方便操作或者需要多次使用相同的表时，可以为表指定别名，用这个别名代替表原来的名称。

为表取别名的基本语法格式为：

```mysql
<表名> [AS] <别名>
```

其中各子句的含义如下：

-  `<表名>`：数据中存储的数据表的名称。 ·
-  `<别名>`：查询时指定的表的新名称。
-  `AS`：关键字为可选参数。

【实例1】为`visits`表取别名 v，输入的 SQL 语句和执行结果如下所示。

```mysql
mysql> select v.ip_counts from visits as v;
+-----------+
| ip_counts |
+-----------+
|         1 |
|         2 |
|         2 |
+-----------+
3 rows in set
```

在使用 SELECT 语句显示查询结果时，MySQL 会显示每个 SELECT 后面指定输出的列，在有些情况下，显示的列名称会很长或者名称不够直观，MySQL 可以指定列的别名，替换字段或表达式。

为列取别名的基本语法格式为： 

```mysql
 <列名> [AS] <列别名>
```

其中，各子句的语法含义如下： 

-  `<列名>`：为表中字段定义的名称。
-  `<列别名>`：字段新的名称。
-  `AS`：关键字为可选参数。

