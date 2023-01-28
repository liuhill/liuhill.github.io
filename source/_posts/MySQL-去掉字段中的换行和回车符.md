---
title: MySQL 去掉字段中的换行和回车符
date: 2017-05-22 10:39:21
tags:
  - mysql
  - 命令行
categories:
  - 技术

---
解决方法：
```
          UPDATE tablename SET field = REPLACE(REPLACE(field, CHAR(10), ''), CHAR(13), '');
          char(10):  换行符
          char(13):  回车符
```
MySQL的trim函数没办法去掉回车和换行，只能去掉多余的空格，可以用MySQL的replace函数，解决掉这个问题，具体解决办法如下：

假设想要审核数据库中内容为“我爱你
”的短信息（注意内容后有换行）通过（status改变成1）

之前的SQL语句是不起作用的

```
UPDATE `tran`
SET `status` = '1'
WHERE `msg` = '我爱你';
```
修改之后的语句
```
UPDATE `tran`
SET `status` = '1'
WHERE trim( replace( `msg`, '\r\n', ' ' ) ) = '我爱你';
```

把数据中的回车换行等替换成空格之后再trim掉，就达到目的了，虽然不是特别完美，但是由于没办法在用户录入的时候控制，所以只能出此下策，好在MySQL内置函数的效率还是很有保证的。
```
UPDATE `tran`
SET `status` = '1'
WHERE trim( trim(
BOTH '\r\n'
FROM content ) ) = '我爱你'
```
用了两个trim，这样的好处是不会替换内容中间的换行和回车，只会处理头尾的空格换行回车，相当于php中trim函数的作用了。
