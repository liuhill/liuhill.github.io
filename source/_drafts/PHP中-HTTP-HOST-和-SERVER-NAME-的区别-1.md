---
title: PHP中 HTTP_HOST 和 SERVER_NAME 的区别
id: 66
categories:
  - php
tags:
---

最近在开发站群软件，用到了根据访问域名判断子站点的相关问题。PHP获取当前域名有两个变量 HTTP_HOST 和 SERVER_NAME，我想知道两者的区别以及哪个更加可靠。

首先我想说，百度上那些转来转去的文章都是扯淡！
有说相同的，有说不同的，都没说明原因，没经过验证就互相转来转去的，浪费观众时间。

下面说说本人经过亲自验证 + 查阅官方文档 + 官方BUG列表 + 官方邮件列表 + sitepoint + webmasterworld + google的总结：

相同点：
当满足以下三个条件时，两者会输出相同信息。

*   1.  服务器为80端口
*   1.  apache的conf中ServerName设置正确
*   1.  HTTP/1.1协议规范

不同点：
1\. 通常情况：
    _SERVER["HTTP_HOST"] 在HTTP/1.1协议规范下，会根据客户端的HTTP请求输出信息。
    _SERVER["SERVER_NAME"] 默认情况下直接输出apache的配置文件httpd.conf中的ServerName值。

1.  当服务器为非80端口时：
_SERVER["HTTP_HOST"] 会输出端口号，例如：mimiz.cn:8080
_SERVER["SERVER_NAME"] 会直接输出ServerName值
因此在这种情况下，可以理解为：HTTP_HOST = SERVER_NAME : SERVER_PORT

2.  当配置文件httpd.conf中的ServerName与HTTP/1.0请求的域名不一致时：
httpd.conf配置如下：

<virtualhost *>
ServerName mimiz.cn
ServerAlias www.mimiz.cn
</virtualhost>

客户端访问域名www.mimiz.cn

    _SERVER["HTTP_HOST"] 输出 www.mimiz.cn
    _SERVER["SERVER_NAME"] 输出 mimiz.cn

所以，在实际程序中，应尽量使用_SERVER["HTTP_HOST"] ，比较保险和可靠。