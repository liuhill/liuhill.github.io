---
title: docker mysql Access denied for user
date: 2017-03-06 12:38:28
tags:
    - docker
categories:
  - 技术

---
```
root@docker:~/drupal# docker inspect drupal_mysql | grep MYSQL_ROOT_PASSWORD
"MYSQL_ROOT_PASSWORD=test",
root@docker:~/drupal# docker exec -it drupal_mysql mysql -u root -p
Enter password: test
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
```

I've gratuitously rebuilt, recreated, `--no-cache'd`, `--force-recreate'd`.. nothing is working. I've also tried putting the environment variable directly in a mysql Dockerfile and as an "environment" argument in my docker-compose.yml.

The only thing that IS working is passing -e 'MYSQL_ROOT_PASSWORD=test' in a docker run statement.
