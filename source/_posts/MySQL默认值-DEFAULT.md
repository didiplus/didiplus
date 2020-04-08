---
title: MySQL默认值(DEFAULT)
tags:
  - Mysql
categories: Mysql
keywords: MySQL添加默认值、修改默认值、删除默认值
description: MySQL添加默认值、修改默认值、删除默认值
cover: 'https://tva3.sinaimg.cn/large/9fc55f55ly1gctglmapvuj20sg0sg7kz.jpg'
top_img: 'https://tva1.sinaimg.cn/large/9fc55f55ly1gctgmapg5lj20sg0b3asm.jpg'
abbrlink: bfc9a30b
date: 2020-03-14 14:24:25
copyright:
---

“默认值（Default）”的完整称呼是“默认值约束（Default Constraint）”。MySQL默认值约束用来指定某列的默认值。

# 创建表是设置默认值约束

创建表时可以使用 ``DEFAULT`` 关键字设置默认值约束，具体的语法规则如下：

```mysql
<字段名> <数据类型> DEFAULT <默认值>;
```

【实例1】创建数据表`student`,指定性别默认值为"男"，输入的 SQL 语句和运行结果如下所示。

```mysql
mysql> create table student(
    -> id int(11) primary key auto_increment,
    -> name varchar(100),
    -> sex varchar(10) default '男',
    -> address varchar(100)
    -> )ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;
Query OK, 0 rows affected
mysql> desc student;
+---------+--------------+------+-----+---------+----------------+
| Field   | Type         | Null | Key | Default | Extra          |
+---------+--------------+------+-----+---------+----------------+
| id      | int(11)      | NO   | PRI | NULL    | auto_increment |
| name    | varchar(100) | YES  |     | NULL    |                |
| sex     | varchar(10)  | YES  |     | 男      |                |
| address | varchar(100) | YES  |     | NULL    |                |
+---------+--------------+------+-----+---------+----------------+
4 rows in set
```

# 在修改表时添加默认值约束

修改表时添加默认值约束的语法规则如下： 

``` mysql
ALTER TABLE <数据表名>
CHANGE COLUMN <字段名> <数据类型> DEFAULT <默认值>;
```

【实例2】修改数据表`student`，将性别的的默认值修改为 '女'，输入的 SQL 语句和运行结果如下所示。

```mysql
mysql> alter table student
    -> change column sex
    -> sex varchar(10) default '女';
Query OK, 0 rows affected
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc student;
+---------+--------------+------+-----+---------+----------------+
| Field   | Type         | Null | Key | Default | Extra          |
+---------+--------------+------+-----+---------+----------------+
| id      | int(11)      | NO   | PRI | NULL    | auto_increment |
| name    | varchar(100) | YES  |     | NULL    |                |
| sex     | varchar(10)  | YES  |     | 女      |                |
| address | varchar(100) | YES  |     | NULL    |                |
+---------+--------------+------+-----+---------+----------------+
4 rows in set

mysql> 
```

# 删除默认值约束

修改表时删除默认值约束的语法规则如下： 

```mysql
修改表时删除默认值约束的语法规则如下：

ALTER TABLE <数据表名>
CHANGE COLUMN <字段名> <字段名> <数据类型> DEFAULT NULL;
```

【实例3】修改数据表 ``student``，将性别位置的默认值约束删除，输入的 SQL 语句和运行结果如下所示。

```mysql
mysql> alter table student
    -> change column sex
    -> sex varchar(10) default null;
Query OK, 0 rows affected
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc student;
+---------+--------------+------+-----+---------+----------------+
| Field   | Type         | Null | Key | Default | Extra          |
+---------+--------------+------+-----+---------+----------------+
| id      | int(11)      | NO   | PRI | NULL    | auto_increment |
| name    | varchar(100) | YES  |     | NULL    |                |
| sex     | varchar(10)  | YES  |     | NULL    |                |
| address | varchar(100) | YES  |     | NULL    |                |
+---------+--------------+------+-----+---------+----------------+
4 rows in set

mysql> 
```

