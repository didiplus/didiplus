---
title: MySQL检查约束(CHECK)
tags:
  - Mysql
categories: Mysql
keywords: mysql检查约束 CHECK
description: mysql检查约束 CHECK
cover: 'https://tvax1.sinaimg.cn/large/9fc55f55ly1gcs3s6f0z3j20xc0m9x6p.jpg'
abbrlink: e2afda08
date: 2020-03-13 09:57:09
top_img:
copyright:
---

MySQL检查约束(CHECK)可以通过`CREATE`或者`ALTER TABLE` 语句实现，根据用户实际的完整性要求来定义。它可以分别对列或表实施 CHECK 约束。

# 选取设置检查约束的字段

检查约束使用 ``CHECK``关键字，具体的语法格式如下：

```mysql
CHECK  <表达式>
```

> 其中：`<表达式>`指的就是 SQL 表达式，用于指定需要检查的限定条件。
>
> 若将 CHECK 约束子句置于表中某个列的定义之后，则这种约束也称为基于列的 CHECK 约束。

# 在创建表是设置检查约束

创建表时设置检查约束的语法规则如下：

```mysql
字段名 类型  CHECK(<检查约束>)
```

【实例1】在`test`数据库中创建`test`数据表，要求 `status`字段值 1 或者0，输入的 SQL 语句和运行结果如下所示。

```mysql
mysql> create table test(
    -> status varchar(1) check(status=1 or status=0)
    -> )ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;
Query OK, 0 rows affected
```

为验证，这个约束是否起作用，我们现在就可以向表中插入数据。

```mysql
mysql> insert into test value(5);
Query OK, 1 row affected

mysql> select * from test;
+--------+
| status |
+--------+
| 5      |
+--------+
1 row in set

mysql> 
```

通过以上可以清楚看到这个status可以正常插入的

**为什么呢**

这也是mysql的一个bug，在官方文档中也有解释，我们不必担心这个问题

这里大家可以看到我们使用的是枚举。



