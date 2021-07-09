---
title: svn merge 合并单个文件
date: 2017-02-04 18:24:01
tags:
---

I'm not sure exactly what you're asking as the title talks about merging single files but the text of the question talks about single revisions. In the case of merging single revisions you need: (to merge the changes committed in revisions 100, 105, 115)

```
cd trunk
svn merge -c 100 -c 105 -c 115 http://..../branches/mybranch .
```
If you want to merge only the part of revision 100 that affects file.cpp:
```
cd trunk/path/to/file.cpp
svn merge -c 100 http://../branches/mybranch/path/to/file.cpp file.cpp
```
