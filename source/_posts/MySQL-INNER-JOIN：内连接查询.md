---
title: MySQL INNER JOIN：内连接查询
tags:
  - Mysql
categories: Mysql
keywords: MySQL INNER JOIN
description: MySQL INNER JOIN内连接查询
cover: 'https://tvax2.sinaimg.cn/large/9fc55f55gy1gd9qhtbvf1j20sf0g27wh.jpg'
abbrlink: 6ecc6510
date: 2020-03-28 15:53:54
top_img:
copyright:
---

内连接是通过在查询中设置连接条件的方式，来移除查询结果集中某些数据行后的交叉连接。简单来说，就是利用条件表达式来消除交叉连接的某些数据行。

在MySQL FROM 子句中使用关键字 INNER JOIN 连接两张表，并使用 ON 子句来设置连接条件。如果没有任何条件，INNER JOIN 和 CROSS JOIN 在语法上是等同的，两者可以互换。

语法格式如下:

```mysql
SELECT <列名1，列名2 …>
FROM <表名1> INNER JOIN <表名2> [ ON子句]
```

语法说明如下。 

-  `<列名1，列名2…>`：需要检索的列名。
-  `<表名1><表名2>`：进行内连接的两张表的表名

内连接是系统默认的表连接，所以在 FROM 子句后可以省略 INNER 关键字，只用关键字 JOIN。使用内连接后，FROM 子句中的 ON 子句可用来设置连接表的条件。

 创建两个张表tcount_tbl和runoob_tbl。如下：

【实例1】创建两张tcount_tbl和runoob_tbl表，并插入测试数据。

```mysql
mysql> create table tcount_tb1(
    -> runoob_author varchar(50),
    -> runoob_count int(10)
    -> );
Query OK, 0 rows affected

mysql> insert into tcount_tb1 values('菜鸟教程',10),('RUNOOB.COM',20),('Google',10)

mysql> create table runoob_tbl(
    -> runoob_id int(10),
    -> runoob_title varchar(50),
    -> runoob_author varchar(50),
    -> submission_date varchar(50)
    -> );
Query OK, 0 rows affected

mysql> insert into runoob_tbl values(1,'学习 PHP','菜鸟教程','2017-04-12'),
(2,'学习 MYSQL','菜鸟教程','2017-04-12'),
(3,'学习 JAVA','RUNOOB.COM','2017-04-12'),
(4,'学习 PYTHON','RUNOOB.COM','2017-04-12')
```

【实例2】使用MySQL的**INNER JOIN(也可以省略 INNER 使用 JOIN，效果一样)**来连接以上两张表来读取runoob_tbl表中所有runoob_author字段在tcount_tbl表对应的runoob_count字段值：

```mysql
mysql> select a.runoob_id, a.runoob_title,b.runoob_count
    -> from runoob_tbl as a inner join tcount_tb1 as b
    -> on a.runoob_author = b.runoob_author;
+-----------+--------------+--------------+
| runoob_id | runoob_title | runoob_count |
+-----------+--------------+--------------+
|         1 | 学习 PHP     |           10 |
|         2 | 学习 MYSQL   |           10 |
|         3 | 学习 JAVA    |           20 |
|         4 | 学习 PYTHON  |           20 |
+-----------+--------------+--------------+
```

以上 SQL 语句等价于：

```mysql
mysql> select a.runoob_id,a.runoob_title,b.runoob_count
    -> from runoob_tbl as a, tcount_tb1 as b
    -> where a.runoob_author = b.runoob_author;
+-----------+--------------+--------------+
| runoob_id | runoob_title | runoob_count |
+-----------+--------------+--------------+
|         1 | 学习 PHP     |           10 |
|         2 | 学习 MYSQL   |           10 |
|         3 | 学习 JAVA    |           20 |
|         4 | 学习 PYTHON  |           20 |
+-----------+--------------+--------------+
4 rows in set

```

![](https://www.runoob.com/wp-content/uploads/2014/03/img_innerjoin.gif)

