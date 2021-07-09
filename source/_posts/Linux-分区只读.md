---
title: Linux 分区只读
date: 2017-08-01 13:31:25
tags:
---

1
down vote
In case it is a fixed drive and not a removable drive, you can add the entry permanently.

```
sudo vi /etc/fstab
```
Add an entry in the following format:
```
<file-system> <mount-point> <type> <options> <dump> <pass>
```
And then do:
```
mount -a
```


mount -o remount,rw /
