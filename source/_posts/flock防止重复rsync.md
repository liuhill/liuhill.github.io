---
title: flock防止重复rsync
date: 2017-02-24 15:26:43
tags:
---
我使用crontab同步一个文件夹时，发现一个问题，我在crontab中设置的1分钟运行一次.但当那个文件夹的内容改变时。1分钟不一定能同步完，但这时第二个rsync进行又起来了。

这个就产生一个问题，二个rsync一起处理相同的文件，这样会出问题。如下

    * * * * /usr/bin/rsync -avlR /data/files    172.16.xxx.xxx:/data

本来想写个脚本来解决，但太麻烦。所以用了个linux下的锁。。呵呵，象下面这个.

    1 * * * * flock -xn /var/run/rsync.lock -c "rsync -avlR /data/files    172.16.xxx.xxx:/data"

这样,使用flock的`-x`参数先建一个锁文件，然后`-n`指定，如果锁存在，就等待。直到建成功锁在会运行-c后面的命令。这样第一个进程没有运行完前，锁文件都会存在。这样就不会二个rsync同时并发处理一个东西了

`来源`: http://www.cnblogs.com/cmsd/p/3697049.html
