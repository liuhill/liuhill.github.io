---
title: mysqldump –ignore-databases
date: 2018-02-22 15:18:13
tags:
---
With mysqldump it is not possible by the parameters to exclude individual databases. However, the database can be easily queried from the information_schema and this makes an exclude.
```
mysqldump --databases
  `mysql --skip-column-names
    -e "SELECT GROUP_CONCAT(schema_name SEPARATOR ' ')
        FROM information_schema.schemata
        WHERE schema_name NOT IN ('mysql','performance_schema','information_schema','db_test');"`
>/dump.sql
```
