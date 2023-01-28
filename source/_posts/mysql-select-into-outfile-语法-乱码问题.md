---
title: mysql select into outfile 语法 乱码问题
date: 2017-12-05 12:53:58
tags:
  - mysql 
  - 命令行
categories:
  - 技术

---
一个常见的问题，mysql 导出csv格式的语法，已经乱码问题：
由于数据库一般默认的是UTF-8 格式的字符集，而execl默认的是gbk格式的字符集，这里有两种方法解决乱码：
#### 方法一： 先转出.txt格式的文件，然后选择用excel打开时，提示选择哪种编码打开，选择gbk即可
```
select * from mobile_order_region where school_id=6921 into outfile '/tmp/6921.txt'
```

#### 方法二：导出时直接设置字符集格式：
```
mysql> select * from mobile_order_region where school_id=6921 into outfile '/tmp/6921.csv'
CHARACTER SET gbk
FIELDS TERMINATED BY ','
OPTIONALLY ENCLOSED BY '"'
 LINES TERMINATED BY '\n';
Query OK, 6888 rows affected (0.11 sec)


mysql> \q
```
