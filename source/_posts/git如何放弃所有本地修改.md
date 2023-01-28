---
title: git如何放弃所有本地修改
date: 2016-11-21 11:50:29
tags:
categories:
  - 技术

---

```
git checkout . #本地所有修改的。没有的提交的，都返回到原来的状态
git stash #把所有没有提交的修改暂存到stash里面。可用git stash pop回复。
git reset --hard HASH #返回到某个节点，不保留修改。
git reset --soft HASH #返回到某个节点。保留修改
```

git丢弃本地修改的所有文件（新增、删除、修改）

```
git clean -df #返回到某个节点
git clean 参数
    -n 显示 将要 删除的 文件 和  目录
    -f 删除 文件
    -df 删除 文件 和 目录
```

也可以使用
```
git checkout . && git clean -xdf
```