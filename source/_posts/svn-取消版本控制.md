---
title: svn 取消版本控制
date: 2017-06-06 17:05:17
tags: 
    - 版本管理
    - svn
categories:
  - 技术
---
删除这些目录是很简单的，命令如下

    find . -type d -name ".svn"|xargs rm -rf
或者

    find . -type d -iname ".svn" -exec rm -rf {} \;  
