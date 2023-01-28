---
title: Shell 前后台进程切换
date: 2017-03-23 09:19:29
tags:
    - 命令上
    
categories:
  - 技术
---
Shell 前后台进程切换

当你用 shell 启动一个程序时，往往他是在前台工作的。比如执行命令

    redis-server
启动 redis 后，shell 就不能再输入了。因为此时 redis 在前台。

Shell 支持作用控制，有以下命令：

- command & 让进程在后台运行
- jobs –l 查看后台运行的进程
- fg %n 让后台运行的进程 n 到前台来
- bg %n 让进程 n 到后台去;

`PS：n 为 jobs 查看到的进程编号。`

### 1、执行命令 & 切换至后台

在 Mac 终端运行命令的时候，在命令末尾加上 & 符号，就可以让程序在后台运行

### 2、切换正在运行的程序到后台

如果程序正在前台运行，可以使用 Ctrl+z 选项把程序暂停，然后用 bg %[number] 命令把这个程序放到后台运行，这个步骤分为3步，如下：

#### 2.1 暂停程序运行Ctrl+z

Ctrl+z 跟系统任务有关的，Ctrl+z 可以将一个正在前台执行的命令放到后台，并且暂停。
```
➜ /Users/apple git:(master) ✗> redis-server

^Z
[1]  + 18669 suspended  redis-server
```
#### 2.2 查看暂停的程序

察看 jobs 使用 jobs 或 ps 命令可以察看正在执行的 jobs

```
➜ /Users/apple git:(master) ✗> jobs -l
[1]  + running    redis-server
```
- jobs 命令执行的结果，+ 表示是一个当前的作业，减号表示是当前作业之后的一个作业。

- jobs -l 选项可显示所有任务的 PID，jobs 的状态可以是 running， stopped， Terminated

#### 2.3 切换程序至后台

bg 将一个在后台暂停的命令，变成继续执行如果后台中有多个命令，可以用 bg %jobnumber 将选中的命令调出.

```
➜ /Users/apple git:(source) ✗> bg
[1]  + 18823 continued  redis-server
➜ /Users/apple git:(source) ✗> jobs -l
[1]  + 18823 running    redis-server
```

#### 2.4 切换程序至前台

也可以用 `fg %[number]` 指令把一个程序掉到前台运行
```
➜ /Users/apple git:(source) ✗> fg %1
[1]  + 18823 running    redis-server
```
#### 2.5 终止后台程序

也可以直接终止后台运行的程序，使用 kill 命令
```
➜ /Users/apple git:(source) ✗> kill %1
```
但是如果任务被终止了 kill，shell 从当前的 shell 环境已知的列表中删除任务的进程标识;也就是说，jobs 命令显示的是当前 shell 环境中所起的后台正在运行或者被挂起的任务信息。
