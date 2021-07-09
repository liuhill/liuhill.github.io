---
title: MongoDB安装
id: 16
categories:
  - 技术文章
date: 2014-09-01 11:10:32
tags:
---

### MongoDB安装

    wget -c http://fastdl.mongodb.org/linux/mongodb-linux-i686-2.4.1.tgz ;
    tar zxvf mongodb-src-r2.4.1.tar.gz
    mv mongodb-linux-i686-2.4.1 /usr/local/mongodb/
    /usr/local/mongodb/bin/mongod --dbpath=/usr/local/mongodb/data/ --logpath=/usr/local/mongodb/dblogs --fork


### 启动以后可以查看mongodb进程树

    pstree -p |grep mongod


### 可能出现的问题：

问题一：

    -bash: /usr/local/bin/mongo: /lib/ld-linux.so.2: bad ELF interpreter: 没有那个文件或目录


解决办法：

    yum install ld-linux.so.2


问题二：

    mongo: error while loading shared libraries: libstdc++.so.6: cannot open shared object file: No such file or directory


解决办法：

    yum whatprovides libstdc++.so.6
    yum install llibstdc++.so.6


  #### PHP MongoDB 扩展

    cd /usr/local/src
    wget -c http://pecl.php.net/get/mongo-1.4.4.tgz
    tar -xzvf ./mongo-1.4.4.tgz
    cd ./mongo-1.4.4
    /usr/local/php/bin/phpize # 利用PHP的 phpize 命令来安装扩展
    ./configure --with-php-config=/usr/local/php/bin/php-config
    make &amp;&amp; make install


完成后，编辑你 php.ini 文件增加一行。

    extension=mongo.so

#### 注意

关闭注意 这里禁止使用 kill -9 PID 关闭mongodb进程，会导致mongod.lock导致再一次无法开启mongodb，必须删除mongod.lock再能开启 可以使用pkill mongod 或者 killall mongod 来结束mongodb进程 出处：http://www.crackedzone.com 完成后，请编辑你php.ini文件增加一行 extension=mongo.so 一般默认的编译php的ini文件在 /usr/local/php/etc/php.ini 重启你的web服务器或者php-fpm，打印phpinfo，如果看到mongo项表，那么mongodb的扩展安装成功了
