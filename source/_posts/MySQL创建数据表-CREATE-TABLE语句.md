---
title: MySQL创建数据表(CREATE TABLE语句)
tags:
  - Mysql
categories: Mysql
keywords: Mysql创建数据表 CREATE TABLE
description: Mysql创建数据表 CREATE TABLE
cover: 'https://tvax2.sinaimg.cn/large/9fc55f55ly1gchnfdherpj20dw08pq32.jpg'
abbrlink: ba34c15b
date: 2020-03-04 09:21:07
top_img:
copyright:
---

# 基本语法

在MySQL中，可以使用```CREATE TABLE```语句创建表。其语法格式：

```MYSQL
CREATE TABLE <表名> ([表定义选项])[表选项][分区选项];
```

> CREATE TABLE 命令语法比较多，其主要是由表创建定义（create-definition）、表选项（table-options）和分区选项（partition-options）所组成的。

# 在指定的数据库中创建表

数据表属于数据库，在创建数据表之前，应使用```USE<数据库>```指定操作在哪个数据库中进行，如果没有选择数据库，就会抛出```No databases seleted```的错误

实例1   创建员工tb_emp1,结构如下表

| 字段名称 | 数据类型    | 备注         |
| -------- | ----------- | ------------ |
| id       | INT(ll)     | 员工编号     |
| name     | VARCHAR(25) | 员工名称     |
| deptld   | INT(ll)     | 所在部门编号 |
| salary   | FLOAT       | 工资         |

选择创建表的数据库 test_db，创建 tb_emp1 数据表，输入的 SQL 语句和运行结果如下所示。

```mysql
mysql> use test_db;
Database changed
mysql> create table tb_emp1
    -> (
    -> id int(11) comment '员工编号',
    -> name varchar(25) comment '员工名称',
    -> depId int(11) comment '所在部门编号',
    -> salary float comment '工资'
    -> );
Query OK, 0 rows affected

mysql> 
```

语句执行后，便创建了一个名称为 tb_emp1 的数据表，使用 SHOW TABLES；语句查看数据表是否创建成功，如下所示。

```MYSQL
mysql> show tables;
+-------------------+
| Tables_in_test_db |
+-------------------+
| tb_emp1           |
+-------------------+
1 row in set

mysql> 
```

# 查看表结构

在 MySQL 中，使用 SQL 语句创建好数据表之后，可以查看结构的定义，以确认表的定义是否正确。在 MySQL 中，查看表结构可以使用 ```DESCRIBE``` 和 ```SHOW CREATE TABLE``` 语句。

```MYSQL
DESCRIBE <表名>; 简写 DESC <表名>
```

使用 DESCRIBE 查看表 tb_emp1 的结构，输入的 SQL 语句和运行结果如下所示。

```mysql
mysql> desc tb_emp1;
+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| id     | int(11)     | YES  |     | NULL    |       |
| name   | varchar(25) | YES  |     | NULL    |       |
| depId  | int(11)     | YES  |     | NULL    |       |
| salary | float       | YES  |     | NULL    |       |
+--------+-------------+------+-----+---------+-------+
4 rows in set

mysql> 
```

>其中，各个字段的含义如下： 
>
>-  Null：表示该列是否可以存储 NULL 值。
>-  Key：表示该列是否已编制索引。PRI 表示该列是表主键的一部分，UNI 表示该列是UNIQUE 索引的一部分，MUL 表示在列中某个给定值允许出现多次。
>-  Default：表示该列是否有默认值，如果有，值是多少。
>-  Extra：表示可以获取的与给定列有关的附加信息，如 AUTO_INCREMENT 等。

SHOW CREATE TABLE语句可以用来显示创建表时的CREATE TABLE语句，语法格式如下

```MYSQL
SHOW CREATE TABLE <表名>\G；
```

