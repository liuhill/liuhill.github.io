---
title: Git忽略已经提交过一次文件
date: 2022-04-13 17:57:46
tags: git 
---
### 一、平时忽略未提交的文件
从未提交过的文件可以用`.gitignore`
也就是添加之后从来没有提交（commit）过的文件，可以使用`.gitignore`忽略该文件
该文件只能作用于未跟踪的文件（Untracked Files），也就是那些从来没有被 git 记录过的文件
在`.gitignore`中写忽略文件或文件夹。注意相对路径是项目根数据，文件格式如下
```
/.gradle/
/.idea/
/local.properties
```

### 二、已经Push过的文件
需要删除Git仓库的文件，本址的文件需要保留，并且在以后提交中忽略

1、从本地Git缓存中删除文件
```
//忽略文件 aaa.txt
git rm --cached aaa.txt
//忽略文件夹 abc
git rm -r --cached abc
```

2、提交到仓库里面
```
git commit -m 'delete somefile'
git push
```

3、修改文件.gitignore，把刚才删除的文件或者文件夹加入.gitignore文件
```
/aaa.txt
/abc/
```