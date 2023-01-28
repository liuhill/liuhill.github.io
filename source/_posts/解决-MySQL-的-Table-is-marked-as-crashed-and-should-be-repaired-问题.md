---
title: 解决 MySQL 的 Table is marked as crashed and should be repaired 问题
date: 2018-01-09 15:52:21
categories:
  - 技术
tags:
    - mysql
    - 数据恢复
---
来源: [https://www.vpsee.com/2013/08/how-to-fix-mysql-table-is-marked-as-crashed-and-should-be-repaird/]

昨天一位 VPS 客户说他的 WordPress 博客没了，网站可以打开，但是文章都没了，怀疑被黑。我们登陆客户 VPS 后没发现被黑迹象，然后进入 MySQL 数据库发现 Table ‘./wordpress/wp_posts’ is marked as crashed and should be repaired 错误，因为 wp_posts 表被损坏了，所以 WordPress 的文章都显示不出来：

```
mysql -u root -p
Enter password:

mysql> use wordpress;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> select * from wp_posts;
ERROR 145 (HY000): Table './wordpress/wp_posts' is marked as crashed and should be repaired
mysql> Bye
```


修复 MySQL 数据库数据表问题可以由 mysqlcheck 来解决，先用 mysqlcheck 查看一下：
```
# mysqlcheck -u root -p wordpress
Enter password:
```

然后添加 –auto-repair 参数自动修复，最好修复前备份一下数据库：

```
# mysqldump -u root -p wordpress > wordpress.sql
Enter password:

# mysqlcheck -u root -p wordpress --auto-repair
Enter password:
wordpress.wp_commentmeta
error    : Table upgrade required. Please do "REPAIR TABLE `wp_commentmeta`" or dump/reload to fix it!
wordpress.wp_comments
error    : Table upgrade required. Please do "REPAIR TABLE `wp_comments`" or dump/reload to fix it!
wordpress.wp_links
error    : Table upgrade required. Please do "REPAIR TABLE `wp_links`" or dump/reload to fix it!
wordpress.wp_options
error    : Table upgrade required. Please do "REPAIR TABLE `wp_options`" or dump/reload to fix it!
wordpress.wp_postmeta
error    : Table upgrade required. Please do "REPAIR TABLE `wp_postmeta`" or dump/reload to fix it!
wordpress.wp_posts
error    : Table upgrade required. Please do "REPAIR TABLE `wp_posts`" or dump/reload to fix it!
wordpress.wp_term_relationships                OK
wordpress.wp_term_taxonomy
error    : Table upgrade required. Please do "REPAIR TABLE `wp_term_taxonomy`" or dump/reload to fix it!
wordpress.wp_terms
error    : Table upgrade required. Please do "REPAIR TABLE `wp_terms`" or dump/reload to fix it!
wordpress.wp_usermeta
error    : Table upgrade required. Please do "REPAIR TABLE `wp_usermeta`" or dump/reload to fix it!
wordpress.wp_users
error    : Table upgrade required. Please do "REPAIR TABLE `wp_users`" or dump/reload to fix it!

Repairing tables
wordpress.wp_commentmeta                       OK
wordpress.wp_comments                          OK
wordpress.wp_links                             OK
wordpress.wp_options                           OK
wordpress.wp_postmeta                          OK
wordpress.wp_posts                             OK
wordpress.wp_term_taxonomy                     OK
wordpress.wp_terms                             OK
wordpress.wp_usermeta                          OK
wordpress.wp_users                
```
