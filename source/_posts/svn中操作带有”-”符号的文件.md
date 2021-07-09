---
title: svn中操作带有”@”符号的文件
date: 2016-11-14 14:56:44
tags:
---
如果在命终端中用svn命令操作文件名中有“@”符号的文件（ios开发的朋友应该很清楚），会遇到：

    svn: E205000: Syntax error parsing peg revision ‘3x.png’
或者：

    svn: E200009: ‘welcome1@3x.png': a peg revision is not allowed here
类似的错误提示，原因是因为svn命令把文件名中的@识别成有其它含义的特殊字符，导致操作失败。

### 解决办法：
在使用svn命令时在文件名尾部再添加一个`@`即可解决。
比如 要查看welcome1@3x.png文件信息：

    svn info welcome1@3x.png@
