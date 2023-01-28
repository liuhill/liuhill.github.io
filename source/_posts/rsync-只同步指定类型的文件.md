---
title: rsync 只同步指定类型的文件
date: 2017-02-15 10:27:09
tags:
  - rsync
  - 命令行
categories:
  - 技术
---
需求： 同步某个目录下所有的图片（*.jpg），该目录下有很多其他的文件，但只想同步*.jpg的文件。

rsync 有一个--exclude 可以排除指定文件，还有个--include选项的作用正好和--exclude相反。
那直接使用--include="*.jpg"是否可以呢？

rsync -av  --include="*.jpg"  /src/   /des/

实验证明，这样是不对的。

而正确的答案是
rsync -av  --include="*.jpg" --exclude=*  /src/ /des/
