---
title: DenyHosts
id: 27
categories:
  - 技术文章
date: 2014-09-30 13:14:59
tags:
---

### DenyHosts介绍

DenyHosts是Python语言写的一个程序，它会分析sshd的日志文件（/var/log/secure），当发现重 复的攻击时就会记录IP到/etc/hosts.deny文件，从而达到自动屏IP的功能。

### DenyHosts应用

当你的linux服务器暴露在互联网之中，该服务器将会遭到互联网上的扫描软件进行扫描，并试图猜测SSH登录口令。

你会发现，每天会有多条SSH登录失败纪录。那些扫描工具将对你的服务器构成威胁，你必须设置复杂登录口令，并将尝试多次登录失败的IP给阻止掉，让其在一段时间内不能访问该服务器。

用DenyHosts可以阻止试图猜测SSH登录口令，它会分析/var/log/secure等日志文件，当发现同一IP在进行多次SSH密码尝试时就会记录IP到/etc/hosts.deny文件，从而达到自动屏蔽该IP的目的。