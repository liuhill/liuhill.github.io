---
title: MySQL按中文拼音排序
date: 2024-05-31 11:54:03
categories:
tags:
---

若数据库字符集为GBK可以直接排序,其他字符集中文需要转码为GBK格式后才可以排序，`ORDER BY CONVERT(user_name USING gbk)`
在MYSQL中想要对字段进行中文规则的排序，常用以下SQL：

```
SELECT * FROM sys_user ORDER BY CONVERT(user_name USING gbk)
```
