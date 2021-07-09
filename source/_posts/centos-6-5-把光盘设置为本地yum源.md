---
title: centos 6.5 把光盘设置为本地yum源
date: 2017-12-20 11:52:13
tags:
---
为了搞学习内核编译，需要安装一些开发工具包，索性把光盘镜像设置成本地yum源，这样更快些！以下是一些基本步骤：

 - 1、 首先挂载光盘到/mnt/cd下，`mount  /dev/cdrom  /mnt/cd`，这个就不多说了；

 - 2、 进入``/etc/yum.repos.d/` `目录下，把原来的yum源备份，我给它重新命名了。

        mv CentOS-Vault.repo CentOS-Vault.repo.bak
        mv CentOS-Debuginfo.repo CentOS-Debuginfo.repo.bak
        mv CentOS-Base.repo CentOS-Base.repo.bak

- 3、新建一个光盘源

`vim  CentOS-Media.repo`

```
# CentOS-Media.repo
#
#  This repo can be used with mounted DVD media, verify the mount point for
#  CentOS-6.  You can use this repo and yum to install items directly off the
#  DVD ISO that we release.
#
# To use this repo, put in your DVD and use it with the other repos too:
#  yum --enablerepo=c6-media [command]
#  
# or for ONLY the media repo, do this:
#
#  yum --disablerepo=\* --enablerepo=c6-media [command]

[c6-media]
name=CentOS-$releasever - Media
baseurl=file:///mnt/cd
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
```
保存退出



- 4、`yum clean all` (清除缓存)

- 5、`yum  makecache` (建立新缓存)
