---
title: 'MySQL LEFT/RIGHT JOIN:外连接查询'
tags:
  - MySQL
categories: Mysql
keywords: left join、right join  外连接查询
description: left join、right join  外连接查询
abbrlink: 2d2af903
date: 2020-03-30 11:52:51
cover: https://tvax4.sinaimg.cn/large/9fc55f55gy1gdd636rs0kj20sf0iye5i.jpg
top_img:
copyright:
---

MySQL中[内连接](https://huafeng28.gitee.io/blog/post/6ecc6510.html)是在交叉连接的结果上返回满足条件的记录；而外连接先将连接的表分为基表和参考表，再以基表为依据返回满足和不满足条件的记录。

外连接更加注重两张表之间的关系。按照连接表的顺序，可以分为左外连接和右外连接。

左外连接又称为左连接，在 `FROM` 子句中使用关键字` LEFT OUTER JOIN` 或者 `LEFT JOIN`，用于接收该关键字左表（基表）的所有行，并用这些行与该关键字右表（参考表）中的行进行匹配，即匹配左表中的每一行及右表中符合条件的行。

【实例1】先在数据`test`中创建两个表` tb_students_info`和`tb_departments`,并插入测试数据。输入的 SQL 语句和执行结果如下所示。

```mysql
mysql> create table tb_departments(
    -> dept_id int(11) not null primary key auto_increment,
    -> dept_name varchar(100)
    -> )ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;
Query OK, 0 rows affected

mysql> create table tb_students_info(
    -> id int(11) not null primary key auto_increment,
    -> name varchar(100),
    -> sex varchar(10),
    -> dept_id int(11),
    -> foreign key(dept_id) references tb_departments(dept_id)
    -> )ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;
Query OK, 0 rows affected

mysql> insert into tb_departments (dept_name) values ('Computer'),('Chinese'),('Math'),('Economy'),('History');
Query OK, 5 rows affected
Records: 5  Duplicates: 0  Warnings: 0

mysql> insert into tb_students_info (name,sex,dept_id) values
    -> ('Dany','male',1),('Jane','male',2),
    -> ('Jim','male',4),('Henry','female',3),
    -> ('John','female',3),('Susan','female',Null);
Query OK, 6 rows affected
Records: 6  Duplicates: 0  Warnings: 0
```

【实例2】在` tb_students_info` 表和 `tb_departments` 表中查询所有学生，包括没有学院的学生，输入的 SQL 语句和执行结果如下所示。

```mysql
mysql> select name,dept_name 
    -> from tb_students_info as s
    -> left outer join tb_departments as d
    -> on s.dept_id = d.dept_id;
+-------+-----------+
| name  | dept_name |
+-------+-----------+
| Dany  | Computer  |
| Jane  | Chinese   |
| Jim   | Economy   |
| Henry | Math      |
| John  | Math      |
| Susan | NULL      |
+-------+-----------+
6 rows in set
```

结果显示了 6 条记录，name 为 Susan的学生目前没有学院，因为对应的 tb_departments 
表中并没有该学生的学院信息，所以该条记录只取出了 tb_students_info 表中相应的值，而从 tb_departments 
表中取出的值为 NULL。



右外连接又称为右连接，在 FROM 子句中使用 RIGHT OUTER JOIN 或者 RIGHT JOIN。与左外连接相反，右外连接以右表为基表，连接方法和左外连接相同。在右外连接的结果集中，除了匹配的行外，还包括右表中有但在左表中不匹配的行，对于这样的行，从左表中选择的值被设置为NULL。

【实例3】在 tb_students_info 表和 tb_departments 表中查询所有学院，包括没有学生的学院，输入的 SQL 语句和执行结果如下所示。

```mysql
mysql> select name,dept_name 
    -> from tb_students_info s
    -> right outer join tb_departments d
    -> on s.dept_id = d.dept_id;
+-------+-----------+
| name  | dept_name |
+-------+-----------+
| Dany  | Computer  |
| Jane  | Chinese   |
| Henry | Math      |
| John  | Math      |
| Jim   | Economy   |
| NULL  | History   |
+-------+-----------+
6 rows in set

```

可以看到，结果只显示了6 条记录，名称为 History 的学院目前没有学生，对应的 tb_students_info 表中并没有该学院的信息，所以该条记录只取出了 tb_departments 表中相应的值，而从 tb_students_info 表中取出的值为NULL。



