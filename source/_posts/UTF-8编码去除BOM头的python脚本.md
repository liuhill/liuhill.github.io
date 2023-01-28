---
title: UTF-8编码去除BOM头的python脚本
date: 2016-12-14 09:58:23
tags:
    - 转码
    - python 
categories:
  - 技术

---
> 欢迎转载，但请务必在明确位置注明文章出处！ >http://johnnyshieh.github.io/python/2015/08/02/python-remove-utf8-bom/

前几天我在Windows写完一篇markdown格式的文章，上传到github page后，这个文章的页面把jekyll的YAML头信息也显示出来了，没有像我之前在ubuntu中写的文章一样正常解析YAML头信息，最后谷歌了半天发现原因是在Windows用记事本保存utf-8编码的文件时会默认加上BOM头字符。而jekyll的文档中也明确写明了如果使用UTF-8编码，那么在文件中一定不要出现BOM头字符。

所以我把如何在Windows和linux系统中去除UTF-8的BOM头的方法做个记录。

1. UTF-8 与 UTF-8 + BOM 的区别

下面是我从网上复制过来BOM的简介：

BOM – Byte Order Mark，中文名译作“字节顺序标记”。在UCS 编码中有一个叫做 “Zero Width No-Break Space” ，中文译名作“零宽无间断间隔”的字符，它的编码是 FEFF。而 FFFE 在 UCS 中是不存在的字符，所以不应该出现在实际传输中。UCS 规范建议我们在传输字节流前，先传输字符 “Zero Width No-Break Space”。这样如果接收者收到 FEFF，就表明这个字节流是 Big-Endian 的；如果收到FFFE，就表明这个字节流是 Little- Endian 的。因此字符 “Zero Width No-Break Space” （“零宽无间断间隔”）又被称作 BOM。

UTF-8 不需要 BOM 来表明字节顺序，但可以用 BOM 来表明编码方式。字符 “Zero Width No-Break Space” 的 UTF-8 编码是 EF BB BF。所以如果接收者收到以 EF BB BF （十六进制）开头的字节流，就知道这是 UTF-8编码了。Windows 就是使用 BOM 来标记文本文件的编码方式的。

所以UTF-8 + BOM其实就是在文件的开头加上了 0xEF 0xBB 0xBF 这三个字节,而这个三个字节是一串隐藏字符。

2. 在Linux中如何检测去除BOM头

在Linux中可以用vim很方便地检测、去除BOM头

检测是否有BOM头：

    :set bomb?
去除BOM头：

    :set encoding=utf-8
    :set nobomb
添加BOM头也很简单：

    :set encoding=utf-8
    :set bomb
3. 利用python脚本在window、linux、mac os中去除BOM头

我在Terence的BomSweeper的基础上，用python3完成了一个去除单个文件BOM或者批量去除一个文件夹内多个文件BOM的脚本。原理就是删除文件开头的BOM 3个字节。

大家可以到Github中查看下载源码，项目地址– Utf8BomRemover

在readme中我有写明脚本的使用方法，如果大家对这个脚本有什么建议欢迎留言。
