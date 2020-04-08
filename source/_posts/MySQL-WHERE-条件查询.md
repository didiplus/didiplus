---
title: 'MySQL WHERE:条件查询'
tags:
  - Mysql
categories: Mysql
keywords: Mysql 条件查询  where查询
description: Mysql 条件查询  where查询
cover: 'https://tva3.sinaimg.cn/large/9fc55f55gy1gd512uzi5hj20sf0iydyg.jpg'
abbrlink: dfb807ee
date: 2020-03-24 14:33:55
top_img:
copyright:
---

在使用MySQL SELECT语句时，可以使用WHERE子句来指定查询条件，从 FROM 子句的中间结果中选取适当的数据行，达到数据过滤的效果。

基本语法格式如下：

```mysql
WHERE <查询条件> {<判定运算1>，<判定运算2>，…}
```

其中，判定运算其结果取值为 TRUE、FALSE 和 UNKNOWN。

>判定运算的语法分类如下： 
>
>-  <表达式1>`{=|<|<=|>|>=|<=>|<>|！=}`<表达式2>
>-  <表达式1>`[NOT]LIKE`<表达式2>
>-  <表达式1>`[NOT][REGEXP|RLIKE]`<表达式2>
>-  <表达式1>`[NOT]BETWEEN`<表达式2>`AND`<表达式3>
>-  <表达式1>`IS[NOT]NULL`

# 单一条件查询语句

【实例1】在表`job`中查找`dept_id`为2的数据，输入的 SQL 语句和行结果如下所示。

```mysql
mysql> select name from job where dept_id = '2';
+----------+
| name     |
+----------+
| 产品经理 |
| 全栈开发 |
| 软件测试 |
+----------+
3 rows in set

mysql> 
```

该语句采用了简单的相等过滤，查询一个指定列 dept_id的具体值 2。

【实例2】查询`dept_id`大于2的所有数据，输入的 SQL 语句和执行结果如下所示。

```mysql
mysql> select name from job where dept_id>2;
+----------+
| name     |
+----------+
| 人事专员 |
+----------+
1 row in set

mysql> 
```

# 多条件的查询语句

使用 SELECT 查询时，可以增加查询的限制条件，这样可以使查询的结果更加精确。MySQL 在 WHERE 子句中使用 AND 操作符限定只有满足所有查询条件的记录才会被返回。

可以使用 AND 连接两个甚至多个查询条件，多个条件表达式之间用 AND 分开。

【实例3】在`job`表中查找`dept_id`为2的，并且`name`以`产品`开头的所有数据，输入的 SQL 语句和执行结果如下所示。

```mysql
mysql> select name from job where dept_id =2 and name like '产品%';
+----------+
| name     |
+----------+
| 产品经理 |
+----------+
1 row in set

mysql> 
```

# 使用LIKE的模糊查询

字符串匹配的语法格式如下：

```mysql
<表达式1> [NOT] LIKE <表达式2>
```

字符串匹配是一种模式匹配，使用运算符 LIKE 设置过滤条件，过滤条件使用通配符进行匹配运算，而不是判断是否相等进行比较。

> 相互间进行匹配运算的对象可以是 CHAR、VARCHAR、TEXT、DATETIME 等数据类型。运算返回的结果是 TRUE 或 FALSE。

## 百分号(%)

百分号是 MySQL 中常用的一种通配符，在过滤条件中，百分号可以表示任何字符串，并且该字符串可以出现任意次。

使用百分号通配符要注意以下几点：

- MySQL 默认是不区分大小写的，若要区分大小写，则需要更换字符集的校对规则。
- 百分号不匹配空值。
- 百分号可以代表搜索模式中给定位置的 0 个、1 个或多个字符。
- 尾空格可能会干扰通配符的匹配，一般可以在搜索模式的最后附加一个百分号。

## 下划线 (_)

下划线通配符和百分号通配符的用途一样，下画线只匹配单个字符，而不是多个字符，也不是 0 个字符。

> 注意：不要过度使用通配符，对通配符检索的处理一般会比其他检索方式花费更长的时间。

【实例4】在`job`表中，查询所有以`开发`结尾的所有数据，输入的 SQL 的语句和执行结果如下所示。

```mysql
mysql> select name from job where name like '%开发';
+----------+
| name     |
+----------+
| 全栈开发 |
+----------+
1 row in set
```



