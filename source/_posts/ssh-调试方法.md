---
title: ssh 调试方法
date: 2017-07-11 10:39:45
tags:
      - 命令行
      - ssh
      - 调式
categories:
  - 技术
---
1. 服务端

      /usr/sbin/sshd -d

-  客户端
      ssh -v -p22 root@192.168.56.10
