---
title: '如何查看apache,php,mysql的编译参数'
id: 162
categories:
  - 技术
date: 2015-03-20 13:41:11
tags:
  - lnmp
  - 命令行
  - 编译
---

查看nginx编译参数：`/usr/local/nginx/sbin/nginx -V`

查看apache编译参数：`cat /usr/local/apache2/build/config.nice`

查看mysql编译参数：`cat /usr/local/mysql/bin/mysqlbug | grep CONFIGURE_LINE`

查看php编译参数：`/usr/local/php/bin/php -i | grep configure`