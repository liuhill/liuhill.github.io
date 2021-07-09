---
title: MySQL 字符串拼接
date: 2017-09-27 10:43:00
tags:
---
在Mysql 数据库中存在两种字符串连接操作.具体操作如下

一. 语法:

   1. CONCAT(string1,string2,…)   说明 : string1,string2代表字符串,concat函数在连接字符串的时候，只要其中一个是NULL,那么将返回NULL

   例1:



   例2:


   2. CONCAT_WS(separator,str1,str2,...)

   说明 : string1,string2代表字符串,concat_ws 代表 concat with separator,第一个参数是其它参数的分隔符。分隔符的位置放在要连接的两个字符串之间。分隔符可以是一个字符串，也可以是其它参数。如果分隔符为 NULL，则结果为 NULL。函数会忽略任何分隔符参数后的 NULL 值。

   举例1:
```
   mysql> select concat_ws('#','dbdh=','NorthEastTrcoon',null) AS dbdh_name_three;
+-----------------------+
| dbdh_name_three       |
+-----------------------+
| dbdh=#NorthEastTrcoon |
+-----------------------+
1 row in set (0.00 sec)
```
  例2:
```
mysql> select concat_ws(null,'dbdh=','NorthEastTrcoon',null) AS dbdh_name_fourth
;
+------------------+
| dbdh_name_fourth |
+------------------+
| NULL             |
+------------------+
1 row in set (0.00 sec)
```
  例3:
```
     mysql> select concat_ws('*','dbdh=','NorthEastTrcoon',null) AS dbdh_name_fifth;
+-----------------------+
| dbdh_name_fifth       |
+-----------------------+
| dbdh=*NorthEastTrcoon |
+-----------------------+
1 row in set (0.00 sec)
```


3. MySQL中group_concat函数
完整的语法如下：
    group_concat([DISTINCT] 要连接的字段 [Order BY ASC/DESC 排序字段] [Separator '分隔符'])

基本查询

```
mysql> select * from stu1;
+------+------+
| id| name |
+------+------+
|1 | 10|
|1 | 20|
|1 | 20|
|2 | 20|
|3 | 200   |
|3 | 500   |
+------+------+
6 rows in set (0.00 sec)
```

以id分组，把name字段的值打印在一行，逗号分隔(默认)

```
mysql> select id,group_concat(name) from aa group by id;
+------+--------------------+
| id| group_concat(name) |
+------+--------------------+
|1 | 10,20,20|
|2 | 20 |
|3 | 200,500|
+------+--------------------+
3 rows in set (0.00 sec)
```

以id分组，把name字段的值打印在一行，分号分隔

```
mysql> select id,group_concat(name separator ';') from aa group by id;
+------+----------------------------------+
| id| group_concat(name separator ';') |
+------+----------------------------------+
|1 | 10;20;20 |
|2 | 20|
|3 | 200;500   |
+------+----------------------------------+
3 rows in set (0.00 sec)
```

以id分组，把去冗余的name字段的值打印在一行，

逗号分隔

```
mysql> select id,group_concat(distinct name) from aa group by id;
+------+-----------------------------+
| id| group_concat(distinct name) |
+------+-----------------------------+
|1 | 10,20|
|2 | 20   |
|3 | 200,500 |
+------+-----------------------------+
3 rows in set (0.00 sec)
```

以id分组，把name字段的值打印在一行，逗号分隔，以name排倒序

```
mysql> select id,group_concat(name order by name desc) from aa group by id;
+------+---------------------------------------+
| id| group_concat(name order by name desc) |
+------+---------------------------------------+
|1 | 20,20,10   |
|2 | 20|
|3 | 500,200|
+------+---------------------------------------+
3 rows in set (0.00 sec)
```


还有一个简单的连接方式为: ||


```
mysql> select *  from student;
+----+------+-------+----------+------------+
| id | age  | score | name     | birth      |
+----+------+-------+----------+------------+
|  1 |   23 |    78 | 李四     | 2017-10-10 |
|  2 |   24 |    78 | zhangsan | 2017-10-10 |
|  3 |   25 |    99 | 王五     | 2016-05-17 |
+----+------+-------+----------+------------+
3 rows in set (0.00 sec)
```
```
mysql> select id+999,name,name+99,name+'999' from student;
+--------+----------+---------+------------+
| id+999 | name     | name+99 | name+'999' |
+--------+----------+---------+------------+
|   1000 | 李四     |      99 |        999 |
|   1001 | zhangsan |      99 |        999 |
|   1002 | 王五     |      99 |        999 |
+--------+----------+---------+------------+
3 rows in set, 6 warnings (0.00 sec)
```
