---
title: MySQL创建数据库(CREATE DATABASE语句)
tags:
  - Mysql
categories: Mysql
keywords: MySQL创建数据库
description: 使用CREATE DATABASE
cover: 'http://tvax4.sinaimg.cn/large/9fc55f55gy1gcdgxsllv3j20e6080mx8.jpg'
abbrlink: 3d972e7d
date: 2020-02-29 12:46:12
top_img:
copyright:
---

### 创建数据库基本语法

在 MySQL中，可以使用**CREATE DATABASE**语句创建数据库，语法格式如下：

```mysql
 CREATE DATABASE [IF NOT EXISTS] <数据库名>
 [[DEFAULT] CHARACTER SET <字符集名>] 
 [[DEFAULT] COLLATE <校对规则名>]; 
```

`[ ]`中的内容是可选的。语法说明如下：
-  <数据库名>：创建数据库的名称。MySQL 的数据存储区将以目录方式表示 MySQL 数据库，因此数据库名称必须符合操作系统的文件夹命名规则，不能以数字开头，尽量要有实际意义。注意在 MySQL 中不区分大小写。
-  IF NOT EXISTS：在创建数据库之前进行判断，只有该数据库目前尚不存在时才能执行操作。此选项可以用来避免数据库已经存在而重复创建的错误。
-  [DEFAULT] CHARACTER SET：指定数据库的字符集。指定字符集的目的是为了避免在数据库中存储的数据出现乱码的情况。如果在创建数据库时不指定字符集，那么就使用系统的默认字符集。
-  [DEFAULT] COLLATE：指定字符集的默认校对规则。



> MySQL 的字符集（CHARACTER）和校对规则（COLLATION）是两个不同的概念。字符集是用来定义 MySQL 存储字符串的方式，校对规则定义了比较字符串的方式。后面我们会单独讲解 MySQL 的字符集和校对规则。

###  实例详解

#### 实例1：最简单的创建 MySQL 数据库的语句

 在 MySQL 中创建一个名为 test_db 的数据库。在 MySQL 命令行客户端输入 SQL 语句`CREATE DATABASE test_db;`

```mysql
 mysql> CREATE DATABASE test_db;
 Query OK, 1 row affected (0.12 sec);
```

若再次输入`CREATE DATABASE test_db;`语句，则系统会给出错误提示信息，如下所示：

```mysql
 mysql> CREATE DATABASE test_db;
 ERROR 1007 (HY000): Can't create database 'test_db'; database exists
```

 提示不能创建“test_db”数据库，数据库已存在。MySQL 不允许在同一系统下创建两个相同名称的数据库

可以加上`IF NOT EXISTS`从句，就可以避免类似错误，如下所示：

```mysql
 mysql> CREATE DATABASE IF NOT EXISTS test_db;
 Query OK, 1 row affected (0.12 sec)
```

####  实例2：创建 MySQL 数据库时指定字符集和校对规则

 使用 MySQL 命令行工具创建一个测试数据库，命名为 test_db_char，指定其默认字符集为 utf8，默认校对规则为 utf8_chinese_ci（简体中文，不区分大小写），输入的 SQL 语句与执行结果如下所示： 

```mysql
 mysql> CREATE DATABASE IF NOT EXISTS test_db_char
     -> DEFAULT CHARACTER SET utf8
     -> DEFAULT COLLATE utf8_chinese_ci;
 Query OK, 1 row affected (0.03 sec)
```

 这时，可以使用`SHOW CREATE DATABASE`

```
mysql> SHOW CREATE DATABASE test_db_char;
+--------------+-----------------------------------------------------+
| Database     | Create Database                                     |
+--------------+-----------------------------------------------------+
| test_db_char | CREATE DATABASE `test_db_char` /*!40100 DEFAULT CHARACTER SET utf8 */ |
+--------------+-----------------------------------------------------+
1 row in set (0.00 sec)
```