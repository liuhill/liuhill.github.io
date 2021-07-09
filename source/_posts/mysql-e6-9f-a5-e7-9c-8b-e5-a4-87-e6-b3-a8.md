---
title: mysql 查看备注
id: 240
categories:
  - server
date: 2015-10-06 14:23:24
tags:
---

## 简单的用法

    SELECT
    COLUMN_NAME,
    COLUMN_COMMENT  AS  `备注`
    FROM information_schema.COLUMNS WHERE TABLE_NAME='pw_admin_role'


### 复杂的用法

    SELECT
    COLUMN_NAME,
    DATA_TYPE AS `数据类型`,
    CHARACTER_MAXIMUM_LENGTH  AS `字符长度`,
    NUMERIC_PRECISION AS `数字长度`,
    NUMERIC_SCALE AS `小数位数`,
    IS_NULLABLE AS `是否允许非空`,
    CASE WHEN EXTRA = 'auto_increment' THEN 1 ELSE 0 END AS `是否自增`,
    COLUMN_DEFAULT  AS  `默认值`,
    COLUMN_COMMENT  AS  `备注`
    FROM information_schema.COLUMNS WHERE TABLE_NAME='pw_admin_role'
