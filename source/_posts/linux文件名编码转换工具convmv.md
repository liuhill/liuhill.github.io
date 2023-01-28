---
title: linux文件名编码转换工具convmv
date: 2017-03-07 17:07:49
tags:
  - 命令行
  - 转码
categories:
  - 技术

---
今天介绍个文件名转码的工具–convmv，convmv能帮助我们很容易地对一个文件，一个目录下所有文件进行编码转换，比如gbk转为utf8等。
### 语法：
    convmv [options] FILE(S) … DIRECTORY(S)
### 主要选项：
- 1、-f ENCODING
指定目前文件名的编码，如-f gbk
- 2、-t ENCODING
指定将要转换成的编码，如-t utf-8
- 3、-r
递归转换目录下所有文件名
- 4、–list
列出所有支持的编码
- 5、–notest
默认是只打印转换后的效果，加这个选项才真正执行转换操作。

- 更多选项请man convmv。

例子：
递归转换centos目录下的目前文件名编码gbk为utf-8:

    convmv -f gbk -t utf-8 --notest -r  centos
