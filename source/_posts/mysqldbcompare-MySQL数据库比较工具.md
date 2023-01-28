---
title: mysqldbcompare MySQL数据库比较工具
date: 2018-08-06 15:40:16
tags:
  - mysql
  - 备份
categories:
  - 技术

---
mysqldbcompare用于比较两个服务器或同个服务器上的数据库，有文件和数据，并生成差异性SQL语句。

要比较数据表，请用另外一个工具：mysqldiff（点击查看教程）。

以下是mysqldbcompare的用法。

## 1 安装
mysqldbcompare是MySQL Utilities中的一个脚本，默认的MySQL不包含工具集，所以需要独立安装。

MySQL Utilities下载地址：http://downloads.mysql.com/archives/utilities/。
Windows系统中需提前安装“Visual C++ Redistributable Packages for Visual Studio 2013”，下载地址：https://www.microsoft.com/en-gb/download/details.aspx?id=40784。
Linux系统在下载页面选择对应发行版。

## 2 语法
mysqldbcompare的语法如下：
```
$ mysqldbcompare --server1=user:pass@host:port:socket --server2=user:pass@host:port:socket db1:db2
```
以上参数中：
```
--server1：MySQL服务器1配置。
--server2：MySQL服务器2配置。如果是同一服务器，--server2可以省略。
db1:db2：要比较的两个数据库。如果比较不同服务器上的同名数据库，可以省略:db2。
--all：比较所有两服务器上所有的同名数据库。--exclude排除无需比较的数据库。
--run-all-tests：运行完整比较，遇到第一次差异时不停止。
--changes-for=：修改对象。例如--changes-for=server2，那么对比以sever1为主，生成的差异的修改也是针对server2的对象的修改。
-d DIFFTYPE,--difftype=DIFFTYPE：差异的信息显示的方式，有[unified|context|differ|sql]，默认是unified。如果使用sql，那么就直接生成差异的SQL，这样非常方便。
--show-reverse：在生成的差异修改里面，同时会包含server2和server1的修改。
--skip-table-options：保持表的选项不变，即对比的差异里面不包括表名、AUTO_INCREMENT、ENGINE、CHARSET等差异。
--skip-diff：跳过对象定义比较检查。所谓对象定义，就是CREATE语句()里面的部分，--skip-table-options是()外面的部分。
--skip-object-compare：默认情况下，先检查两个数据库中相互缺失的对象，再对都存在对象间的差异。这个参数的作用就是，跳过第一步，不检查相互缺失的对象。
--skip-checksum-table：数据一致性验证时跳过CHECKSUM TABLE。
--skip-data-check：跳过数据一致性验证。
--skip-row-count：跳过字段数量检查。
```
## 3 示例
比较两个数据库，并生成差异SQL：
```
$ mysqldbcompare --server1=root:root@localhost --server2=root:root@localhost db1:db2 --changes-for=server1 -a --difftype=sql

# WARNING: Objects in server1.db1 but not in server1.db2:
# TABLE: table2
#
# WARNING: Objects in server1.db2 but not in server1.tb1:
# TABLE: table3
#
#                                                   Defn    Row     Data
# Type      Object Name                             Diff    Count   Check
#-------------------------------------------------------------------------
# TABLE     t1                                      pass    pass    -
#           - Compare table checksum                                FAIL
#           - Find row differences                                  FAIL
#
# Transformation for --changes-for=server1:
#

# Data differences found among rows:
UPDATE db1.t1 SET b = 'Test 123' WHERE a = '1';
UPDATE db1.t1 SET b = 'Test 789' WHERE a = '3';
DELETE FROM db1.t1 WHERE a = '4';
INSERT INTO db1.t1 (a, b) VALUES('5', 'New row - db2');


# Database consistency check failed.
#
# ...done
```
WARNING之后提示两个数据库表之间的差异，也就是一个数据库中有，另一个数据库没有的数据表。

之后就是差异的SQL语句了，把有#号注释的行删掉，就能直接在数据库中执行了。

`说明`：执行MySQL语句时可能会遇到这样错误：`Error 1054 - Unknown column 'name' in 'aspect'``

这是因为mysqldbcompare生成的ALTER语句中，用逗号,拼装了多条ADD、CHANGE等语句，如果这些语句还包含AFTER关键字，就会提示这个错误并中断执行MySQL语句。解决的办法就是：去除AFTER及其后面的条件。

这可能是MySQL的一个Bug，详情参考：http://bugs.mysql.com/bug.php?id=34972 和 http://bugs.mysql.com/bug.php?id=60650。
