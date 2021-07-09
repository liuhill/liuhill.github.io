---
title: CentOS下将MySQL 5.1升级到MySQL 5.5
id: 316
categories:
  - 未分类
date: 2016-04-01 11:41:38
tags:
---

mysql 5.5已经出来有一段时间，性能有明显提升，特别是对多核CPU的支持与TPS性能的提升。上周博主介绍了linux下编译安装mysql 5.5的步骤，安装不出意外基本没有问题。不过可能很多朋友和我一样一直用的是mysql 5.1，现在想把数据库升级成5.5了。博主根据实际操作，记录这次升级操作。

mysql基础信息

1、安装目录
`[root@vm-199~]# /usr/local/mysql`
2、配置文件
`[root@vm-199~]# /etc/my.cnf`
3、数据目录
`[root@vm-199~]# /data/mysql`
4、启动脚本
`[root@vm-199~]# /etc/init.d/mysql`

备份数据和安装、配置文件

`[root@vm-199~]# mysqldump -uroot -p –all-databases </root/zhangnq/mysql5.1/mysql_dbk_20140217.sql
[root@vm-199~]# tar czvf mysql_5.1.60_full.tar.gz /usr/local/mysql
[root@vm-199~]# tar czvf mysql_5.1.60_data_full.tar.gz /data/mysql
[root@vm-199~]# cp /etc/my.cnf  ./`

数据备份好后关闭mysql数据库，`/etc/init.d/mysql stop`，删除/usr/local/mysql文件。

安装mysql 5.5

具体可以参考这篇文章《Linux下编译安装Mysql-5.5的简单步骤》（http://www.sijitao.net/1563.html），安装目录、数据目录和5.1的一样，都是/usr/local/mysql 。

更新配置文件

`[root@vm-199 mysql-5.5.35]# cp support-files/my-huge.cnf /etc/my.cnf`

在配置文件中添加数据目录，datadir = /data/mysql 。

启动mysql 5.5，执行更新程序并重启mysql

`[root@vm-199 mysql-5.5.35]# /etc/init.d/mysql start
[root@vm-199 mysql-5.5.35]# /usr/local/mysql/bin/mysql_upgrade
Looking for 'mysql' as: /usr/local/mysql/bin/mysql
Looking for 'mysqlcheck' as: /usr/local/mysql/bin/mysqlcheck
Running 'mysqlcheck' with connection arguments: '--port=3306' '--socket=/tmp/mysqld.sock'
Running 'mysqlcheck' with connection arguments: '--port=3306' '--socket=/tmp/mysqld.sock'
mydb.t1 OK
mydb.t2 OK
mysql.columns_priv OK
mysql.db OK
mysql.event OK
mysql.func OK
mysql.general_log OK
mysql.help_category OK
mysql.help_keyword OK
mysql.help_relation OK
mysql.help_topic OK
mysql.host OK
mysql.ndb_binlog_index OK
mysql.plugin OK
mysql.proc OK
mysql.procs_priv OK
mysql.proxies_priv OK
mysql.servers OK
mysql.slow_log OK
mysql.tables_priv OK
mysql.time_zone OK
mysql.time_zone_leap_second OK
mysql.time_zone_name OK
mysql.time_zone_transition OK
mysql.time_zone_transition_type OK
mysql.user OK
Running 'mysql_fix_privilege_tables'...
OK`
至此mysql已经更新好了。登陆mysql，检查数据是否和原来一样。

这个mysql升级其实不复杂，其实就是重新安装一遍，然后把数据目录文件覆盖一下。不过数据库升级，主要还是得注意数据备份，防止数据和意外丢失。
