---
title: php echo或者exit输出中文
id: 153
categories:
  - 技术
date: 2014-12-19 10:01:06
tags:
  - php
  - 命令行
---

你这是中文乱码，原因是操作环境的编码和浏览器的编码不一致造成的 1\. php文件本身的编码与网页的编码应匹配

a. 如果欲使用gb2312编码，那么php要输出头：header(“Content-Type: text/html; charset=gb2312")，静态页面添加<meta http-equiv="Content-Type" content="text/html; charset=gb2312" />
，所有文件的编码格式为ANSI，可用记事本打开，另存为选择编码为ANSI，覆盖源文件。

b. 如果欲使用utf-8编码，那么php要输出头 ：header(“Content-Type: text/html; charset=utf-8")，静态页面添加<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
，所有文件的编码格式为utf-8。保存为utf-8可能会有点麻烦，一般utf-8文件开头会有BOM，如果使用 session就会出问题，可用editplus来保存，在editplus中，工具->参数选择->文件->UTF-8签名，选择总是删除，再保存就可以去掉BOM信息了。

1.  php本身不是Unicode的，所有substr之类的函数得改成mb_substr（需要装mbstring扩展）；或者用iconv转码。