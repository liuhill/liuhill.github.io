---
title: nginx/php-fpm 访问php文件直接下载而不运行
date: 2016-09-10 22:07:59
tags:
---
一般都是由于nginx的配置文件引起的。注意一下部分`fastcgi_param`,默认为`/script$fastcgi_script_name`,修改为`$document_root$fastcgi_script_name`可以解决
```
location ~ \.php$ {
           root          /data/nginx/www.123.com;
           fastcgi_pass   127.0.0.1:9000;
           fastcgi_index  index.php;
           fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
           include        fastcgi_params;
       }
```
