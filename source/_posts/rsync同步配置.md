---
title: rsync进行网站异地镜像
date: 2016-10-27 09:10:48
tags:
---

站点远程同步脚本，实现数据库本地镜像。

`如果源文件删除，目标文件夹也会删除。因此该脚本只能够作为镜像，不能作为备份`
### rsync.sh 执行脚本

```
#!/bin/sh

path=$1

host=rsync@103.235.220.108
port=2222

dbuser=root
dbpwd=111111


#远程服务器运行dbdump.sh脚本dump数据库
echo "dump msyql"
ssh $host -p $port "~/dbdump.sh"

src=$host:/home/wwwroot/html
dst=/home/wwwroot/html
exculude=/home/wwwroot/rsync_exclude.list

echo "源:"$src/$path
echo "目标:"$dst/$path
rsync -avzO -e "ssh -p $port" --exclude-from=$exculude   --delete  $src/$path  $dst/$path

echo ">>>更新数据库"
CurrentDate=$(date +%Y%m%d)
mysql -u$dbuser -p$dbpwd test < $dst/db/test.${CurrentDate}.sql

echo "同步完成"

```

### rsync_exclude.list
忽略的文件,文件夹路径
```
.svn/
Upload/
logs/
```

### dbdump.sh
```
#!/bin/sh
dbname=root
dbpwd=111111
CurrentDate=$(date +%Y%m%d)
dirPath=/home/wwwroot/html/db
mysqldump -u$dbname -pdbpwd test > $dirPath/test.${CurrentDate}.sql
```

### demo
同步test文件夹

./rsync.sh test

### 显示进度条

添加参数`--progress`

    rsync -av --progress  root@123.123.123.123:/root/mysql56.tar .
