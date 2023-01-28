---
title: CentOS系统iptables如何打开端口和查看规则
id: 64
categories:
  - 技术
date: 2014-11-16 15:40:23
tags:
    - centos
    - iptables
---

CentOS安装好如果希望开放其他端口的话，需要打开所需端口,比如打开http的默认端口80

编辑iptables

    root:vi /etc/sysconfig/iptables


添加

    -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT


重新启动服务

    /sbin/service iptables restart


查看端口是否开放

    /sbin/iptables -L -n
