---
title: MySQL非空约束(NOT NuLL)
tags:
  - Mysql
categories: Mysql
keywords: Mysql非空约束、删除约束、修改约束
description: Mysql非空约束、删除约束、修改约束
cover: 'https://tva4.sinaimg.cn/large/9fc55f55ly1gcux0ypuwfj20sf0iy4qp.jpg'
abbrlink: 7857d14
date: 2020-03-15 20:48:33
top_img:
copyright:
---

MySQL非空约束（NOT NULL）可以通过 CREATE TABLE 或 ALTER TABLE 语句实现。在表中某个列的定义后加上关键字 NOT NULL 作为限定词，来约束该列的取值不能为空。

# 在创建表时设置非空约束

创建表时可以使用 ``NOT NULL`` 关键字设置非空约束，具体的语法规则如下：

```mysql
<字段名> <数据类型> NOT NULL;
```

【实例1】创建数据表`dept`，指定部门名称不能为空，输入的 SQL 语句和运行结果如下所示。

```mysql
mysql> create table emp(
    -> id int(11) primary key auto_increment,
    -> name varchar(100) not null,
    -> msg varchar(100)
    -> );
Query OK, 0 rows affected

mysql> desc emp;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| name  | varchar(100) | NO   |     | NULL    |                |
| msg   | varchar(100) | YES  |     | NULL    |                |
+-------+--------------+------+-----+---------+----------------+
3 rows in set

mysql> 
```

# 删除非空约束

修改时删除非空约束的语法规则如下：

```mysql
ALTER TABLE <数据表名>
CHANGE COLUMN <字段名> <字段名> <数据类型> NULL;
```

【实例2】修改数据表`emp`,将部门名称的非空约束删除，输入的 SQL 语句和运行结果如下所示。

```mysql
mysql> alter table emp
    -> change column name  name varchar(100) null;
Query OK, 0 rows affected
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc emp;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| name  | varchar(100) | YES  |     | NULL    |                |
| msg   | varchar(100) | YES  |     | NULL    |                |
+-------+--------
```

# 在修改表时添加非空约束

修改表时设置非空约束的语法规则如下：

```mysql
ALTER TABLE <数据表名>
CHANGE COLUMN <字段名>
<字段名> <数据类型> NOT NULL;
```

【实例3】修改数据表`emp`，指定部门位置不能为空，输入的 SQL 语句和运行结果如下所示。

```mysql
mysql> alter table emp
    -> change column name name varchar(100) not null;
Query OK, 0 rows affected
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc emp;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| name  | varchar(100) | NO   |     | NULL    |                |
| msg   | varchar(100) | YES  |     | NULL    |                |
+-------+--------------+------+-----+---------+----------------+
3 rows in set

mysql> 
```





