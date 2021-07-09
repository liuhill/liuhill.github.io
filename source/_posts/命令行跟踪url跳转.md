---
title: 命令行跟踪url跳转
date: 2018-03-01 18:20:23
tags:
---

hillock:~ liuligang$ wget -O /dev/null --content-disposition  http://airhubdemo.shareco.cn
--2018-03-01 18:19:17--  http://airhubdemo.shareco.cn/
正在解析主机 airhubdemo.shareco.cn (airhubdemo.shareco.cn)... 119.61.68.138
正在连接 airhubdemo.shareco.cn (airhubdemo.shareco.cn)|119.61.68.138|:80... 已连接。
已发出 HTTP 请求，正在等待回应... 302 Moved Temporarily
位置：https://airhubdemo.shareco.cn [跟随至新的 URL]
--2018-03-01 18:19:17--  https://airhubdemo.shareco.cn/
正在连接 airhubdemo.shareco.cn (airhubdemo.shareco.cn)|119.61.68.138|:443... 已连接。
错误: 无法验证 airhubdemo.shareco.cn 的由 “L=HangZhou,ST=ZheJiang,C=CN,O=H3C,CN=www.h3c.com” 颁发的证书:
  出现了自己签名的证书。
    错误: 证书通用名 “www.h3c.com” 与所要求的主机名 “airhubdemo.shareco.cn” 不符。
要以不安全的方式连接至 airhubdemo.shareco.cn，使用“--no-check-certificate”
