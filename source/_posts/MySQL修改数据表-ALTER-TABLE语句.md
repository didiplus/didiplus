---
title: MySQL修改数据表(ALTER TABLE语句)
tags:
  - Mysql
categories: Mysql
keywords: 修改数据表，ALTER TABLE语句的使用
description: 修改数据表，ALTER TABLE语句的使用
cover: 'https://tva2.sinaimg.cn/large/9fc55f55ly1gck1oj9wxdj20dc07877a.jpg'
abbrlink: e3461340
date: 2020-03-06 10:20:22
top_img:
copyright:
---

在MySQL中可以使用```ALTER TABLE```语句来改变原有表的结构，例如增加或删除减列、创建或取消索引、更改原有列类型、重新命名列或表等。

# 基本语法

修改表指的是修改数据库中已经存在的数据表的结构。MySQL 使用 ```ALTER TABLE``` 语句修改表。常用的修改表的操作有修改表名、修改字段数据类型或字段名、增加和删除字段、修改字段的排列位置、更改表的存储引擎、删除表的外键约束等。

常用的语法格式如下：

```mysql
ALTER TABLE <表名> [修改选项]
```

修改选项的语法格式如下：

```mysql
{ADD COLUMN <列名> <类型>
| CHANGE COLUMN <旧列名> <新列名> <新列类型>
| ALTER COLUMN  <列名>  { SET DEFAULT <默认值> | DROP DEFAULT }
| MODIFY COLUMN <列名> <类型>
| DROP COLUMN <列名>
| RENAME TO <新表名>
}
```

## 添加字段

随着业务的变化，可能需要在已经存在的表中添加新的字段，一个完整的字段包括字段名、数据类型、完整性约束。添加字段的语法格式如下：

```mysql
ALTER TABLE <表名> ADD COLUMN <新字段名> <数据类型> [约束条件] [FIRST|AFTER 已存在的字段名]；
```

>`新字段名`为需要添加的字段的名称；`FIRST` 为可选参数，其作用是将新添加的字段设置为表的第一个字段；`AFTER` 为可选参数，其作用是将新添加的字段添加到指定的`已存在的字段名`的后面。

【实例1】使用ALTER TABLE修改表Student的结构，在表的第一列添加一个int类型的字段`age`，输入的SQL语句和运行结果如下：

```mysql
mysql> alter table Student 
    -> add column s_age varchar(10) after s_name;
Query OK, 0 rows affected
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc Student;
+---------+-------------+------+-----+---------+-------+
| Field   | Type        | Null | Key | Default | Extra |
+---------+-------------+------+-----+---------+-------+
| s_id    | int(11)     | YES  |     | NULL    |       |
| s_name  | varchar(50) | YES  |     | NULL    |       |
| s_age   | varchar(10) | YES  |     | NULL    |       |
| s_birth | varchar(30) | YES  |     | NULL    |       |
| s_sex   | varchar(10) | YES  |     | NULL    |       |
+---------+-------------+------+-----+---------+-------+
5 rows in set

mysql> 
```

## 添加主键

为保证数据的完整性，每个数据表都不会存在重复的数据，所以需要对表添加一些约束条件，例如，主键约束，唯一约束等等。

【实例2】使用`ALTER TABLE`修改表Student的结构，为表添加主键约束，并且设置属性`s_id`自动增长，输入的SQL语句和运行结果如下：

```mysql
mysql> alter table Student
    -> modify column s_id int(11) primary key auto_increment;
Query OK, 0 rows affected
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc Student;
+---------+-------------+------+-----+---------+----------------+
| Field   | Type        | Null | Key | Default | Extra          |
+---------+-------------+------+-----+---------+----------------+
| s_id    | int(11)     | NO   | PRI | NULL    | auto_increment |
| s_name  | varchar(50) | YES  |     | NULL    |                |
| s_age   | varchar(10) | YES  |     | NULL    |                |
| s_birth | varchar(30) | YES  |     | NULL    |                |
| s_sex   | varchar(10) | YES  |     | NULL    |                |
+---------+-------------+------+-----+---------+----------------+
5 rows in set
```

## 修改字段类型

有时候根据业务的需求，会对数据库的某个字段进行修改，例如`Student`中的`S_sex`字段存储是学生的性别，这种情况，只有两种要么是`男`要么是`女`。像这种情况，可以使用枚举类型存储。

【实例3】把`Student`表中的`s_sex`属性类型修改成枚举，输入的SQL语句和运行结果如下：

```mysql
mysql> alter table Student
    -> modify column s_sex enum('男','女');
Query OK, 0 rows affected
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc Student;
+---------+-----------------+------+-----+---------+----------------+
| Field   | Type            | Null | Key | Default | Extra          |
+---------+-----------------+------+-----+---------+----------------+
| s_id    | int(11)         | NO   | PRI | NULL    | auto_increment |
| s_name  | varchar(50)     | YES  |     | NULL    |                |
| s_age   | varchar(10)     | YES  |     | NULL    |                |
| s_birth | varchar(30)     | YES  |     | NULL    |                |
| s_sex   | enum('男','女') | YES  |     | NULL    |                |
+---------+-----------------+------+-----+---------+----------------+
5 rows in set
```

## 删除字段

表中的某个字段已经不需要了，要进行删除操作，可以使用如下命令：

```mysql
ALTER TABLE <表名> DROP COLUMN <列名>
```

【实例4】把`Student`表中的`s_birth`字段删除，输入的SQL语句和运行结果如下：

```mysql
mysql> alter table Student
    -> drop column s_birth;
Query OK, 0 rows affected
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc Student;
+--------+-----------------+------+-----+---------+----------------+
| Field  | Type            | Null | Key | Default | Extra          |
+--------+-----------------+------+-----+---------+----------------+
| s_id   | int(11)         | NO   | PRI | NULL    | auto_increment |
| s_name | varchar(50)     | YES  |     | NULL    |                |
| s_age  | varchar(10)     | YES  |     | NULL    |                |
| s_sex  | enum('男','女') | YES  |     | NULL    |                |
+--------+-----------------+------+-----+---------+----------------+
4 rows in set
```

## 修改表名

MySQL 通过 ALTER TABLE 语句来实现表名的修改，语法规则如下：

```mysql
ALTER TABLE <旧表名> RENAME [TO] <新表名>；
```

其中，`TO` 为可选参数，使用与否均不影响结果。

【实例5】使用 ALTER TABLE 将数据表 `Student`改名为 `Student2`，输入的 SQL 语句和运行结果如下所示

```mysql
mysql> alter table Student rename to Student2;
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

