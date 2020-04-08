---
title: 'Mysql SELECT:数据表查询语句'
tags:
  - Mysql
categories: Mysql
keywords: Mysql数据表查询语句
description: Mysql数据表查询语句
cover: 'https://tvax4.sinaimg.cn/large/9fc55f55ly1gcvm952rrvj20sg0lc49h.jpg'
abbrlink: 8da7dc77
date: 2020-03-16 11:14:04
top_img:
copyright:
---

Mysql表单查询是指从一张表的数据中查询所需的数据，主要有查询所有字段、查询指定字段、查询指定记录、查询空值、多条件的查询、对查询结果进行排序等。

# MySQL SELECT基本语法

MySQL 从数据表中查询数据的基本语句为 SELECT 语句，基本格式如下：

```mysql
SELECT 
{* | <字段列名>}
[
    FROM <表 1>, <表 2>…
    [WHERE <表达式>
    [GROUP BY <group by definition>
    [HAVING <expression> [{<operator> <expression>}…]]
    [ORDER BY <order by definition>]
    [LIMIT[<offset>,] <row count>]
]
```

其中，各条子句含义如下： 

- `{*|<字段列名>}`包含星号通配符的字段列表，表示查询的字段，其中字段列至少包含一个字段名称，如果要查询多个字段，多个字段之间要用逗号隔开，最后一个字段后不要加逗号。
- `FROM <表 1>，<表 2>…`，表 1 和表 2 表示查询数据的来源，可以是单个或多个。
- WHERE 子句是可选项，如果选择该项，将限定查询行必须满足的查询条件。
- `GROUP BY< 字段 >`，该子句告诉 MySQL 如何显示查询出来的数据，并按照指定的字段分组。
- `[ORDER BY< 字段 >]`，该子句告诉 MySQL 按什么样的顺序显示查询出来的数据，可以进行的排序有升序（ASC）和降序（DESC）。
- `[LIMIT[<offset>，]<row count>]`，该子句告诉 MySQL 每次显示查询出来的数据条数。

# 使用“*”查询表中的全部内容

在 SELECT 语句中使用星号“*”通配符查询所有字段。

SELECT 查询记录最简单的形式是从一个表中检索所有记录，实现的方法是使用星号“*”通配符指定查找所有列的名称，语法格式如下：

```mysql
SELECT * FROM 表名
```

【实例2】查询`visits`表中的所有数据，输入的SQL语句和执行结果如下：

```mysql
mysql> select * from visits;
+-----+---------------------+------------+-----------+-----------+----------+
| id  | create_time         | date       | ip_counts | pv_counts | week_day |
+-----+---------------------+------------+-----------+-----------+----------+
| 108 | 2019-10-29 21:45:49 | 2019-10-29 |         1 |         1 | Tue      |
| 109 | 2019-10-30 08:58:54 | 2019-10-30 |         2 |        11 | Wed      |
| 110 | 2019-10-31 09:04:18 | 2019-10-31 |         2 |         8 | Thu      |
+-----+---------------------+------------+-----------+-----------+----------+
3 rows in set

mysql> 
```

由执行结果可知，使用星号“*”通配符时，将返回所有列，数据列按照创建表时的顺序显示。

# 查询表中指定的字段

查询表中的某一个字段的语法格式为： 

```MYSQL
 SELECT < 列名 > FROM < 表名 >;
```

【实例3】查询`visits`数据表中的`ip_counts`字段所有数据，输入的SQL语句和执行结果如下：

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

mysql> 
```

如果需要查询多个字段时，中间用逗号“，”分隔开，最后一个字段后面不需要加逗号，语法格式如下： 

```mysql
SELECT <字段名1>,<字段名2>,…,<字段名n> FROM <表名>;
```



 

