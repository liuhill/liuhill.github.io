---
title: nginx显示PHP 500错误
date: 2022-12-28 16:03:52
categories:
    - 技术
tags:
    - PHP
    - 500
    - nginx
---

### 1 修改 `/usr/local/php/etc/php.ini`
```
display_errors = On
``` 

### 2 在 ` /usr/local/php/etc/php-fpm.conf` 添加
```
[global]
......
error_log = /usr/local/php/var/log/php-fpm.log
......

[www]
catch_workers_output = yes
......
```

### 3 重启`php-fpm`