---
title: MySQL主键（PRIMARY KEY）
tags:
  - Mysql
categories: Mysql
keywords: Mysql主键创建、修改、删除
description: Mysql主键创建、修改、删除
cover: 'https://tva4.sinaimg.cn/large/9fc55f55ly1gcni9bm8rmj20sf0aa0u8.jpg'
abbrlink: fea984c4
date: 2020-03-09 10:31:29
top_img:
copyright:
---

> 主键（PRIMARY KEY），也称“主键约束”。MySQL主键约束时一个列或者列的组合，其值能唯一地标识表中的每一行。这样的一列或多列称为表的主键，通过它可以强制表的实体完整性。

# 选取设置主键约束的字段

主键约束即在表中定义一个主键来唯一确定表中每一行数据的标识符。 

主键可以是表中的某一列或者多列的组合，其中由多列组合的主键称为复合主键。

# 主键规则

每个表有且仅有一个主键。

# 在创建表是设置主键约束

在`CREATE TABLE`语言中，主键是通过`PRIMARY KEY`关键字来指定的。

在定义列的同时指定主键，语法规则如下：

```mysql
<字段名> <数据类型> PRIMARY KEY [默认值]
```

【实例1】在test数据库中创建`student`数据表，其主键为id,输入的SQL语句如下：

```mysql
mysql> create table student
    -> (
    -> id int(11) primary key,
    -> name varchar(50),
    -> age varchar(30)
    -> );
Query OK, 0 rows affected

mysql> desc student;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | NO   | PRI | NULL    |       |
| name  | varchar(50) | YES  |     | NULL    |       |
| age   | varchar(30) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
3 rows in set
```

在定义完所有列之后，指定主键的语法格式为：

```mysql
[CONSTRAINT <约束名>] PRIMARY KEY [字段名]
```

【实例2】在`test`数据库中创建student1数据表，输入的 SQL 语句和运行结果如下：

```mysql
mysql> create table student1
    -> (
    -> id int(11),
    -> name varchar(50),
    -> age varchar(30),
    -> primary key(id)
    -> );
Query OK, 0 rows affected

mysql> desc student1;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | NO   | PRI | NULL    |       |
| name  | varchar(50) | YES  |     | NULL    |       |
| age   | varchar(30) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
3 rows in set

mysql> 
```

# 在创建表时设置复合主键

主键由多个字段联合组成，语法规则如下：

```mysql
PRIMARY KEY [字段1,字段2,...,字段n]
```

【实例3】创建数据表`emp`,假设表中没有主键`id`，为了唯一确定一个员工，可以把`name`、`deptId`联合起来作为主键，输入SQL语句和运行结果如下

```mysql
mysql> create table emp
    -> (
    -> name varchar(50),
    -> depId int(11),
    -> salary float,
    -> primary key(name,depId)
    -> );
Query OK, 0 rows affected

mysql> desc emp;
+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| name   | varchar(50) | NO   | PRI | NULL    |       |
| depId  | int(11)     | NO   | PRI | NULL    |       |
| salary | float       | YES  |     | NULL    |       |
+--------+-------------+------+-----+---------+-------+
3 rows in set

mysql> 
```

# 删除数据表的主键

删除表中的主键的基本语法，如下：

```mysql
alter table <数据表> drop primary key;
```

【实例5】把`student1`中的主键删除，输入的SQL如下：

```mysql
mysql> alter table student1 drop primary key;
Query OK, 0 rows affected
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc student1;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | NO   |     | NULL    |       |
| name  | varchar(50) | YES  |     | NULL    |       |
| age   | varchar(30) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
3 rows in set
```

# 在修改表是添加主键约束

在修改数据表是添加主键约束的基本语法为：

```mysql
ALTER TABLE <数据表名> ADD PRIMARY KEY(<列名>);
```

查看`student1`数据表结构，如下

```mysql
mysql> desc student1;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | NO   |     | NULL    |       |
| name  | varchar(50) | YES  |     | NULL    |       |
| age   | varchar(30) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
3 rows in set

mysql> 
```

【实例4】修改数据表`student1`，将字段`name`设置为主键，输入的SQL如下：

```mysql
mysql> alter table student1
    -> add primary key (name);
Query OK, 0 rows affected
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc student1;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(11)     | NO   |     | NULL    |       |
| name  | varchar(50) | NO   | PRI | NULL    |       |
| age   | varchar(30) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+
3 rows in set

mysql> 
```

