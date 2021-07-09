---
title: 'MYSQL服务无法启动：InnoDB: .\ibdata1 can&#039;t be opened in read-write mode'
id: 168
categories:
  - 技术文章
date: 2015-04-22 13:19:55
tags:
---

今天在那做实验倒腾mysql数据库，后来发现服务无法启动，查看日志报错如下：

2015-01-07 17:48:54 9136 [ERROR] InnoDB: .\ibdata1 can't be opened in read-write mode
2015-01-07 17:48:54 9136 [ERROR] InnoDB: The system tablespace must be writable!
2015-01-07 17:48:54 9136 [ERROR] Plugin 'InnoDB' init function returned error.
2015-01-07 17:48:54 9136 [ERROR] Plugin 'InnoDB' registration as a STORAGE ENGINE failed.
2015-01-07 17:48:54 9136 [ERROR] Unknown/unsupported storage engine: InnoDB
2015-01-07 17:48:54 9136 [ERROR] Aborting

解决方法：
1、打开任务管理器终止mysqld进程；
2、打开mysql安装目录的data文件夹，删除以下2个文件：
ib_logfile0和ib_logfile1

3、重新启动mysql

* * *

Dylan    Presents.