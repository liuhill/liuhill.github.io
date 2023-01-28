---
title: 多个网站使用不同的SSH密钥登陆
date: 2016-09-24 11:20:23
categories:
  - 技术
tags:
   - ssh
   - 密钥
---
1.创建不同的SSH密钥， -t指定加密方法，RSA或DSA；-C注释；-f指定文件名

    ssh-keygen -t dsa -C "email.xxx" -f ~/.ssh/xxx
以上命令在~/.ssh/下生成xxx密钥对


2.编辑 ~/.ssh/config  文件，配置格式示例：
```
Host github.com www.github.com
   IdentityFile ~/.ssh/code_github
Host bitbucket.org www.bitbucket.org
   IdentityFile ~/.ssh/code_bitbucket
```
查看 `man ssh_config` 有更多参数说明

3.把xxx.pub公钥文件的内容加入到要自动登陆服务器指定用户下的`~/.ssh/authorized_keys` 文件中

4.使用ssh登陆网站时会自动判断使用哪一对密钥认证
