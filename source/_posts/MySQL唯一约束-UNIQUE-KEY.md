---
title: MySQL唯一约束(UNIQUE KEY)
tags:
  - Mysql
categories: Mysql
keywords: Mysql 唯一约束  UNIQUE KEY 添加唯一约束，删除唯一约束
description: Mysql 唯一约束  UNIQUE KEY 添加唯一约束，删除唯一约束
cover: 'https://tva1.sinaimg.cn/large/9fc55f55ly1gcr08ughrvj20sf0iykhp.jpg'
top_img: 'https://tva4.sinaimg.cn/large/9fc55f55ly1gcr09h1tpcj20sf0a5dme.jpg'
abbrlink: 76fe01d5
date: 2020-03-11 16:53:06
copyright:
---

MySQL唯一约束（Unique Key）要求该列唯一，允许为空，但只能出现一个空值。唯一约束可以确保一列或者几列不出现重复值。

# 在创建表是设置唯一约束

在定义完列之后直接使用 ``UNIQUE`` 关键字指定唯一约束，语法规则如下：

```mysql
<字段名> <数据类型> UNIQUE
```

【实例1】创建一个学生表`students`，指定学生名字是唯一，输入的 SQL 语句和运行结果如下所示。

```mysql
mysql> create table student(
    -> id int(11) primary key auto_increment,
    -> name varchar(100) unique,
    -> age varchar(10)
    -> )ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;
Query OK, 0 rows affected

mysql> desc student;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| name  | varchar(100) | YES  | UNI | NULL    |                |
| age   | varchar(10)  | YES  |     | NULL    |                |
+-------+--------------+------+-----+---------+----------------+
3 rows in set

mysql> 
```

> 提示：UNIQUE 和 PRIMARY KEY 的区别：一个表可以有多个字段声明为 UNIQUE，但只能有一个 PRIMARY KEY 声明；声明为 PRIMAY KEY 的列不允许有空值，但是声明为 UNIQUE 的字段允许空值的存在。

# 删除唯一约束

在 MySQL 中删除唯一约束的语法格式如下：

```mysql
ALTER TABLE <表名> DROP INDEX <唯一约束名>;
```

【实例2】把学生表`student`的name唯一约束删除，输入的 SQL 语句和运行结果如下所示。

```mysql
mysql> alter table student drop index name;
Query OK, 0 rows affected
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc student;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| name  | varchar(100) | YES  |     | NULL    |                |
| age   | varchar(10)  | YES  |     | NULL    |                |
+-------+--------------+------+-----+---------+----------------+
3 rows in set

mysql> 
```

# 在修改表时创建唯一约束

在修改表时添加唯一约束的语法格式为：

```mysql
ALTER TABLE <数据表名> ADD CONSTRAINT <唯一约束名> UNIQUE(<列名>);
```

【实例3】修改学生表`student`，指定学生的名子唯一，输入的 SQL 语句和运行结果如下所示。

```mysql
mysql> alter table student add constraint name unique(name);
Query OK, 0 rows affected
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc student;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| name  | varchar(100) | YES  | UNI | NULL    |                |
| age   | varchar(10)  | YES  |     | NULL    |                |
+-------+--------------+------+-----+---------+----------------+
3 rows in set
```