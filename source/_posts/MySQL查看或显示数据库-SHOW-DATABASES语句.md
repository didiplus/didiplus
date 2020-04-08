---
title: MySQL查看或显示数据库(SHOW DATABASES语句)
tags:
  - Mysql
categories: Mysql
keywords: show database
description: 查看数据库
cover: 'http://tvax2.sinaimg.cn/large/9fc55f55gy1gcdjv8zy9sj20e8080750.jpg'
abbrlink: 60b68f5d
date: 2020-02-29 20:08:37
top_img:
copyright:
---

### 查看数据库基本语法

在 MySQL 中，可使用 ```SHOW DATABASES``` 语句来查看或显示当前用户权限范围以内的数据库。查看数据库的语法格式为：

```MYSQL
SHOW DATABASES [LIKE '数据库名'];
```

> 语法说明如下： 
>
> - LIKE 从句是可选项，用于匹配指定的数据库名称。LIKE 从句可以部分匹配，也可以完全匹配。
>
> - 数据库名由单引号`' '`包围。

### 案例详解

##### 实例1：查看所有数据库

列出当前用户可以查看的所有数据库：

```mysql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
10 rows in set
```

> 可以发现，在上面的列表中有 4 个数据库，它们都是安装 MySQL 5.7时系统自动创建的，其各自功能如下：
>
> - information_schema：主要存储了系统中的一些数据库对象信息，比如用户表信息、列信息、权限信息、字符集信息和分区信息等
> - mysql：MySQL 的核心数据库，类似于 SQL Server 中的 master 表，主要负责存储数据库用户、用户访问权限等 MySQL 自己需要使用的控制和管理信息。常用的比如在 mysql 数据库的 user 表中修改 root 用户密码。
> - performance_schema：主要用于收集数据库服务器性能参数。

##### 实例2：创建并查看数据库

先创建一个名为test_db的数据库

```mysql
mysql> create database test_db;
Query OK, 1 row affected
mysql > show databases
```

##### 实例3：使用LIKE从句

先创建三个数据库，名字分别是test_db、db_test、db_test_db。

1）使用LIKE从句，查看与test_db完全匹配的数据库；

```mysql
mysql> show databases like 'test_db';
+--------------------+
| Database (test_db) |
+--------------------+
| test_db            |
+--------------------+
1 row in set
```

2) 使用 LIKE 从句，查看名字中包含 test 的数据库：

```mysql
mysql> show databases like '%test%';
+-------------------+
| Database (%test%) |
+-------------------+
| db_test           |
| db_test_db        |
| test              |
| test_db           |
| test_db_char      |
+-------------------+
5 rows in set
```

3) 使用 LIKE 从句，查看名字以 db 开头的数据库：

```mysql
mysql> show databases like "db%";
+----------------+
| Database (db%) |
+----------------+
| db_test        |
| db_test_db     |
+----------------+
2 rows in set
```

4) 使用 LIKE 从句，查看名字以 db 结尾的数据库：

```mysql
mysql> show databases like "%db";
+----------------+
| Database (%db) |
+----------------+
| db_test_db     |
| test_db        |
+----------------+
2 rows in set
```

