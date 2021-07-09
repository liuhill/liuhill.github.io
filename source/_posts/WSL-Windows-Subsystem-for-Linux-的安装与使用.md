---
title: WSL(Windows Subsystem for Linux)的安装与使用
date: 2018-02-28 16:31:22
tags:
---
## 1 按照官网安装手册安装WSL UBUNTU
在windows10 上安装linux系统。具体参考官方文档。
https://docs.microsoft.com/en-us/windows/wsl

## 2 中文支持
启动WSL安装中文包
```
apt-get update
apt-get install language-pack-zh-hans
update-locale LANG=zh_CN.UTF-8
```
重启

## 3 window10 命令行乱码
按下WIN+R快捷键，在运行的窗口中输入:control,打开控制面板；

选择区域-->管理-->非Unicode程序的语言-->更改系统区域设置：中文（简体,中国）

重启电脑，dos中文操作显示，中文输入可以正常使用。
