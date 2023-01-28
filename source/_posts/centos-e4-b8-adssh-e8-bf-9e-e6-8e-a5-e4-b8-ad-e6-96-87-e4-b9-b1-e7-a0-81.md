---
title: CentOS中SSH连接中文乱码
id: 68
categories:
  - 技术
date: 2014-11-17 23:01:48
tags:
  - centos
  - ssh
---

CentOS 6 ，在SSH时回显中文乱码，则：

修改 /etc/sysconfig/i18n 文件

命令：

    vi /etc/sysconfig/i18n
    

    将里面的文字 最终修改为：

    LANG="zh_CN.GB18030"
    LANGUAGE="zh_CN.GB18030:zh_CN.GB2312:zh_CN"
    SUPPORTED="zh_CN.UTF-8:zh_CN:zh:en_US.UTF-8:en_US:en"
    SYSFONT="lat0-sun16"

最后，断开重连SSH就可以了，进入用date命令既可查看效果
