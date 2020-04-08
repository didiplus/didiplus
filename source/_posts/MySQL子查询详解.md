---
title: MySQL子查询详解
tags:
  - Mysql
categories: Mysql
keywords: MySQL子查询详解
description: MySQL子查询详解
cover: 'https://tvax4.sinaimg.cn/large/9fc55f55gy1gdd4a7mxi7j20xc0qox6q.jpg'
abbrlink: 2b8375ce
date: 2020-03-31 15:08:09
top_img:
copyright:
---

子查询指一个查询语句嵌套在另一个查询语句内部的查询，这个特性从  MySQL 4.1 开始引入，在 SELECT 子句中先计算子查询，子查询结果作为外层另一个查询的过滤条件，查询可以基于一个表或者多个表。

子查询中常用的操作符有 ANY（SOME）、ALL、IN 和 EXISTS。

子查询可以添加到 SELECT、UPDATE 和 DELETE 语句中，而且可以进行多层嵌套。子查询也可以使用比较运算符，如“<”、“<=”、“>”、“>=”、“！=”等。

# 子查询中常用的运算符

## IN子查询

结合关键字 IN 所使用的子查询主要用于判断一个给定值是否存在于子查询的结果集中。其语法格式为：

```mysql
<表达式> [NOT] IN <子查询>
```

语法说明如下。 

- `<表达式>`：用于指定表达式。当表达式与子查询返回的结果集中的某个值相等时，返回 TRUE，否则返回 FALSE；若使用关键字 NOT，则返回的值正好相反。
- `<子查询>`：用于指定子查询。这里的子查询只能返回一列数据。对于比较复杂的查询要求，可以使用 SELECT 语句实现子查询的多层嵌套。

## 比较运算符子查询

比较运算符所使用的子查询主要用于对表达式的值和子查询返回的值进行比较运算。其语法格式为：

```mysql
<表达式> {= | < | > | >= | <= | <=> | < > | != }
{ ALL | SOME | ANY} <子查询>
```

语法说明如下。 

- `<子查询>`：用于指定子查询。
- `<表达式>`：用于指定要进行比较的表达式。
- `ALL`、`SOME` 和 `ANY`：可选项。用于指定对比较运算的限制。其中，关键字  ALL 用于指定表达式需要与子查询结果集中的每个值都进行比较，当表达式与每个值都满足比较关系时，会返回 TRUE，否则返回 FALSE；关键字  SOME 和 ANY 是同义词，表示表达式只要与子查询结果集中的某个值满足比较关系，就返回 TRUE，否则返回 FALSE。

## EXIST子查询

关键字 EXIST 所使用的子查询主要用于判断子查询的结果集是否为空。其语法格式为： 

```mysql
 EXIST <子查询>
```

 若子查询的结果集不为空，则返回 TRUE；否则返回 FALSE。 

# 子查询的应用

【实例 1】在 tb_departments 表中查询 dept_type 为 A 的学院 ID，并根据学院 ID 查询该学院学生的名字，输入的 SQL 语句和执行结果如下所示。

```mysql
mysql> select name from tb_students_info  where dept_id in 
    -> (select dept_id from tb_departments where dept_type ='A');
+------+
| name |
+------+
| Dany |
| Jim  |
+------+
2 rows in set
```

上述查询过程可以分步执行，首先内层子查询查出 tb_departments 表中符合条件的学院 ID，单独执行内查询，查询结果如下所示。

```mysql
mysql> select dept_id from tb_departments where dept_type='A';
+---------+
| dept_id |
+---------+
|       1 |
|       4 |
|       5 |
+---------+
3 rows in set
```

可以看到，符合条件的 dept_id 列的值有两个：1 、4和 5。然后执行外层查询，在 tb_students_info 表中查询 dept_id 等于 1 、4 或 2 的学生的名字。嵌套子查询语句还可以写为如下形式，可以实现相同的效果。

```mysql
mysql> select name from tb_students_info where dept_id in (1,4,5);
+------+
| name |
+------+
| Dany |
| Jim  |
+------+
2 rows in set
```

【实例 2】与前一个例子类似，但是在 SELECT 语句中使用 NOT IN 关键字，输入的 SQL 语句和执行结果如下所示

```mysql
mysql> select name from tb_students_info  where dept_id not in
    -> (select dept_id from tb_departments where dept_type='A');
+-------+
| name  |
+-------+
| Jane  |
| Henry |
| John  |
+-------+
3 rows in set
```