---
title: MySQL 导入慢的解决方法
date: 2017-02-09 09:20:22
tags:
  - 数据库
  - mysql
  - 备份
categories:
  - 技术

---
MySQL导出的SQL语句在导入时有可能会非常非常慢，经历过导入仅45万条记录，竟用了近3个小时。在导出时合理使用几个参数，可以大大加快导 入的速度。

-  `-e` 使用包括几个VALUES列表的多行INSERT语法;
- `-max_allowed_packet=XXX` 客户端/服务器之间通信的缓存区的最大大小;
-  `--net_buffer_length=XXX`  TCP/IP和套接字通信缓冲区大小,创建长度达net_buffer_length的行。

`注意`：max_allowed_packet和net_buffer_length不能比目标数据库的设定数值 大，否则可能出错。

首先确定目标库的参数值
```
mysql>show variables like 'max_allowed_packet';
mysql>show variables like 'net_buffer_length';
```
根据参数值书写mysqldump命令，如：

    E:\eis>mysqldump -uroot -p eis_db goodclassification -e --max_allowed_packet=1048576 --net_buffer_length=16384 >good3.sql

之前2小时才能导入的sql现在几十秒就可以完成了。
