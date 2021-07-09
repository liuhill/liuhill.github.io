---
title: 解决LNMP下提示fileinfo插件没安装（PHP安装fileinfo扩展教程）
id: 62
categories:
  - server
date: 2014-11-15 18:08:13
tags:
---

编译并安装fileinfo插件

    cd /root/lnmp1.0-full/php-5.3.17/ext/fileinfo
    /usr/local/php/bin/phpize
    ./configure --with-php-config=/usr/local/php/bin/php-config
    make &amp;&amp; make install
    

    在PHP配置中添加fileinfo插件
    用vim编辑php.ini

    vim /usr/local/php/etc/php.ini
    

    找到 ";extension=php_bz2.dll" 这一行
    在其上面添加一行：

    extension = fileinfo.so
    

    然后重启lnmp

    /root/lnmp restart

结束！
