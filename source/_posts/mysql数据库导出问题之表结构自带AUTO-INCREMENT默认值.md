---
title: mysql数据库导出问题之表结构自带AUTO_INCREMENT默认值
date: 2022-04-13 19:02:57
tags:
---
解决：
```
mysqldump -uroot -p -d test -S /tmp/mysql.sock | sed 's/AUTO_INCREMENT=[0-9]*\s*//g' > test.sql
```