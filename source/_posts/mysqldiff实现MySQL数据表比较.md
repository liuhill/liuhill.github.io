---
title: mysqldiff实现MySQL数据表比较
date: 2018-08-06 15:44:09
tags:
---

本文介绍mysqldiff工具来比较数据表结构，并生成差异SQL语句。

mysqldiff类似Linux下的diff命令，用来比较对象的定义是否相同，并显示不同的地方。

如果要比较数据库是否一致，可以用另外一个工具：mysqldbcompare（点击查看教程）。

以下是mysqldiff的用法。

## 1 安装
mysqldiff是MySQL Utilities中的一个脚本，默认的MySQL不包含这个工具集，所以需要独立安装。

MySQL Utilities下载地址：http://downloads.mysql.com/archives/utilities/。
Windows系统中需提前安装“Visual C++ Redistributable Packages for Visual Studio 2013”，下载地址：https://www.microsoft.com/en-gb/download/details.aspx?id=40784。
Linux系统在下载页面选择对应发行版。

## 2 语法
mysqldiff的语法格式是：
```
$ mysqldiff --server1=user:pass@host:port:socket --server2=user:pass@host:port:socket db1.object1:db2.object1 db3:db4
```
这个语法有两个用法：
```
db1:db2：如果只指定数据库，那么就将两个数据库中互相缺少的对象显示出来，不比较对象里面的差异。这里的对象包括表、存储过程、函数、触发器等。
db1.object1:db2.object1：如果指定了具体表对象，那么就会详细对比两个表的差异，包括表名、字段名、备注、索引、大小写等所有的表相关的对象。
接下来看一些主要的参数：

--server1：配置server1的连接。
--server2：配置server2的连接。
--character-set：配置连接时用的字符集，如果不显示配置默认使用character_set_client。
--width：配置显示的宽度。
--skip-table-options：保持表的选项不变，即对比的差异里面不包括表名、AUTO_INCREMENT、ENGINE、CHARSET等差异。
-d DIFFTYPE,--difftype=DIFFTYPE：差异的信息显示的方式，有[unified|context|differ|sql]，默认是unified。如果使用sql，那么就直接生成差异的SQL，这样非常方便。
--changes-for=：修改对象。例如--changes-for=server2，那么对比以sever1为主，生成的差异的修改也是针对server2的对象的修改。
--show-reverse：在生成的差异修改里面，同时会包含server2和server1的修改。
```
## 3 范例
先创建两个表。

use study;

create table test1(
    id int not null primary key,
    a varchar(10) not null,
    b varchar(10),
    c varchar(10) comment 'c',
    d int
)ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='test1';

create table test2(
    id int not null,
    a varchar(10),
    b varchar(5),
    c varchar(10),
    D int
)ENGINE=myisam DEFAULT CHARSET=utf8 COMMENT='test2';
不使用--skip-table-options，

mysqldiff --server1=root:root@localhost --server2=root:root@localhost --changes-for=server2 --show-reverse --difftype=sql study.test1:study.test2


使用`--skip-table-options`，



如果需要生成SQL文件，加上输出就可以了：
```
mysqldiff --server1=root:root@localhost --server2=root:root@localhost --changes-for=server2 --show-reverse --difftype=sql study.test1:study.test2 > output.sql
```
`说明`：执行MySQL语句时可能会遇到这样错误：`Error 1054 - Unknown column 'name' in 'aspect'``

这是因为mysqldbcompare生成的ALTER语句中，用逗号,拼装了多条ADD、CHANGE等语句，如果这些语句还包含AFTER关键字，就会提示这个错误并中断执行MySQL语句。解决的办法就是：去除AFTER及其后面的条件。

这可能是MySQL的一个Bug，详情参考：http://bugs.mysql.com/bug.php?id=34972 和 http://bugs.mysql.com/bug.php?id=60650。
