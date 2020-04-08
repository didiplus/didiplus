---
title: MySQL外键约束(FOREIGN KEY)
tags:
  - Mysql
categories: Mysql
keywords: Mysql数据库外键、创建外键、修改外键、删除外键
description: Mysql数据库外键、创建外键、修改外键、删除外键
cover: 'https://tva3.sinaimg.cn/large/9fc55f55ly1gcov8mj1aej20jw089jws.jpg'
abbrlink: b32f27
date: 2020-03-10 10:29:29
top_img:
copyright:
---

MySQL外键约束(FOREIGN KEY)用来在两个表的数据之间建立链接，它可以是一列或多列。一个表可以有一个或多个外键。

> 外键对应的是参照完整性，一个表的外键可以为空值，若不为空值，则每一个外键的值必须等于另一个表中主键的某个值。

> 外键是表的一个字段，不是本表的主键，但对应另一个表的主键。定义外键后，不允许删除另一个表中具有关联关系的行。



**Mysql数据库默认使用的引擎是MyISAM，而MyISAM引擎不支持建外键，所以将数据库默认引擎改为InnoDB。**

# 外键的作用

外键的作用是保持数据的一致性、完整性。例如，课程表`course`的主键`c_id`，在学生表`student`中有一个建`courseId`与这个`c_id`关联。

- 主表（父表）：对于两个具有关联关系的表而言，相关联字段中主键所在的表就是主表。
- 从表（子表）：对于两个具有关联关系的表而言，相关联字段中外键所在的表就是从表。

>  简单来说就是，一对多的关系，一是主表，多是从表

#  选取设置 MySQL 外键约束的字段

定义一个外键时，需要遵守下列规则：

1. 父表必须已经存在于数据库中，或者是当前正在创建的表。
2. 必须为父表定义主键
3. 主键不能包含空值，但允许在外键中出现空值。也就是说，只要外键的每个非空值出现在指定的主键中，这个外键的内容就是正确的。
4. 在父表的表名后面指定列名或列名的组合。这个列或列的组合必须是父表的主键或候选键。
5. 外键中列的数目必须和父表的主键中列的数目相同。
6. 外键中列的数据类型必须和父表主键中对应列的数据类型相同。

# 在创建表是设置外键约束

在数据表中创建外键使用 ``FOREIGN KEY 关键字``，具体的语法规则如下：

```mysql
方式1：不指定外键名称，数据库自动生成
FOREIGN KEY (<列名>) REFERENCES <主表名> (<列名>) [ ON UPDATE CASCADE ON DELETE CASCADE]
方式2：方式二：指定外键名称为(FK_Name)
constraint FK_Name FOREIGN KEY (<列名>) REFERENCES <主表名> (<列名>) [ on delete cascade on update cascade ]
```

> 注意：**外键名**为定义的外键约束的名称，一个表中不能有相同名称的外键；**字段名**表示子表需要添加外健约束的字段列；主表名即被子表外键所依赖的表的名称；主键列表示主表中定义的主键列或者列组合。

【实例1】创建学生表`students`和课程表`course`，其中学生表和课程表是一对多的关系，输入的SQL语句如下

```mysql
mysql> create table course(
    -> c_id int(11) primary key auto_increment,
    -> c_name varchar(50),
    -> c_grade varchar(20)
    -> )ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;
Query OK, 0 rows affected
mysql> create table students(
    ->id int auto_increment primary key,
    ->name varchar(6),
    ->class_id int,
    ->foreign key(class_id) references Class(id) ON UPDATE CASCADE ON DELETE CASCADE
)ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci
```

# 删除外键约束

对于数据库中定义的外键，如果不再需要，可以将其删除。外键一旦删除，就会解除主表和从表间的关联关系，MySQL 中删除外键的语法格式如下：

```mysql
ALTER TABLE <表名> DROP FOREIGN KEY <外键约束名>;
```

【实例2】把学生表`students`的外键删除，输入的SQL语句如下

```mysql
mysql> alter table students drop foreign key students_ibfk_1;
Query OK, 0 rows affected
Records: 0  Duplicates: 0  Warnings: 0

mysql> 
```

# 在修改表时添加外键约束

在修改数据表时添加外键约束的语法规则为：

```mysql
ALTER TABLE <数据表名> ADD CONSTRAINT <FK_Name>
FOREIGN KEY(<列名>) REFERENCES <主表名> (<列名>);
```

【实例3】把学生表的`students`添加外键关系，输入的SQL语句如下

```mysql
mysql> alter table students add constraint students_ibfk_1
    -> foreign key (c_course) references course(c_id);
Query OK, 0 rows affected
Records: 0  Duplicates: 0  Warnings: 0

mysql> 
```

