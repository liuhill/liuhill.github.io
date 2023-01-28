---
title: ubuntu 22.04 设置svn server 开机启动
date: 2023-01-03 11:25:03
categories:
    - 技术
tags:
    - ubunt 22.04
    - svn
---
1. 创建svnserve系统服务配置文件`/etc/systemd/system/svnserve.service`：

Create systemd service configuration for svnserve `/etc/systemd/system/svnserve.service`

```
[Unit]
Description=SVN server
After=network.target

[Service]
User=svn
Group=svn
Type=forking
ExecStart=/usr/bin/svnserve -d -r /home/svn

[Install]
WantedBy=multi-user.target
```

> *Note that I set Type=forking because svnserve daemonizes itself with -d.* 

> *请注意，我设置Type=forking是因为svnserve使用-d.*

> *Side note: Your own service definitions should go to /etc/systemd/system/, /lib/systemd/system/ is reserved for unit definitions that come with system packages.*

>*旁注：您自己的服务定义应该转到/etc/systemd/system/，/lib/systemd/system/保留给系统包附带的单元定义。*

2. 启动服务器`sudo systemctl start svnserve.service`

3. 添加到开机启动 `sudo systemctl enable  svnserve.service`
