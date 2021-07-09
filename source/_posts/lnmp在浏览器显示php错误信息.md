---
title: lnmp在浏览器显示php错误信息
date: 2018-02-14 11:13:53
tags:
---
- 1、下载安装lnmp一建安装脚本 https://lnmp.org/
- 2、修改`/usr/local/php/etc/php-fpm.conf`

```
[global]
pid = /usr/local/php/var/run/php-fpm.pid
error_log = /usr/local/php/var/log/php-fpm.log
log_level = notice

[www]
catch_workers_output = yes
listen = /tmp/php-cgi.sock
listen.backlog = -1
listen.allowed_clients = 127.0.0.1
listen.owner = www
listen.group = www
listen.mode = 0666
user = www
group = www
pm = dynamic
pm.max_children = 80
pm.start_servers = 40
pm.min_spare_servers = 40
pm.max_spare_servers = 80
request_terminate_timeout = 100
request_slowlog_timeout = 0
slowlog = var/log/slow.log
php_flag[display_errors] = On

```
