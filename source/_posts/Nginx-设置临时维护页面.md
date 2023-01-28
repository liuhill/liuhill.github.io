---
title: Nginx 设置临时维护页面
date: 2018-01-25 11:39:26
tags:
  - lnmp
  - nginx
categories:
  - 技术

---
nginx维护页面处理-全部URL指向同一个页面
一般来说nginx的维护页面需要把所有访问本站的链接全部重定向到某个指定页面

## 1.rewrite
```
rewrite ^(.*)$ /maintain.html break;
```
注意这句后面如果有重定向等语句，那么后面执行的重定向等语句需要全部注释掉

## 2.使用状态码
```
location /{
return 503;
}
#注意其他location优先级高的匹配均需要注释掉
error_page 503 /maintain.html;
```

每当服务器遇到 502 代码时，就自动转到临时维护的静态页：

```
server {
listen 80;
server_name mydomain.com;

# ... 省略掉 N 行代码


error_page 502 = @tempdown;

location @tempdown {
rewrite ^(.*)$ /pages/maintain.html break;
}
}

```


如果你只想要【临时维护页面】就这样写（适合服务器更新东西或者改版）：

```
server {
listen 80;
server_name mydomain.com;

# ... 省略掉 N 行代码

# 所有页面都转跳到维护页
rewrite ^(.*)$ /pages/maintain.html break;

}
```

*** 注：***
*** 临时维护页要找对正确的路径，我的例子是 http://mydomain.com/page/maintain.html。所以路径是 /page/maintain.html ***
