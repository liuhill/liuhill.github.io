---
title: /bin/wipefs挖矿程序排查
date: 2018-01-03 13:20:36
tags:
    - 木马
    - 挖矿
categories:
  - 技术

---
### 基本情况
```
[root@test ~]# cat /etc/redhat-release
CentOS release 6.5 (Final)
[root@test ~]# uname -a
Linux test 2.6.32-431.el6.x86_64 #1 SMP Fri Nov 22 03:15:09 UTC 2013 x86_64 x86_64 x86_64 GNU/Linux
[root@test ~]# file /bin/wipefs
/bin/wipefs: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), statically linked, stripped
[root@test ~]# ll /bin/wipefs
-rwxr-xr-x 1 root root 2384177 Jul 18 2013 /bin/wipefs
[root@monitor ~]# lsattr /bin/wipefs
----i--------e- /bin/wipefs
[root@monitor ~]# lsattr /bin/ddus-uidgen
----i--------e- /bin/ddus-uidgen

```

### 改动了dns配置

```
[root@test rc3.d]# stat /etc/resolv.conf
File: `/etc/resolv.conf'
Size: 106 Blocks: 8 IO Block: 4096 regular file
Device: 802h/2050d Inode: 1182160 Links: 1
Access: (0644/-rw-r--r--) Uid: ( 0/ root) Gid: ( 0/ root)
Access: 2017-11-22 06:00:02.797144215 +0800
Modify: 2017-11-22 06:00:02.795144215 +0800
Change: 2017-11-22 06:00:02.795144215 +0800
[root@test rc3.d]# cat /etc/resolv.conf
nameserver 208.67.222.222	#加拿大DNS
nameserver 114.114.114.114
nameserver 208.67.222.222
nameserver 114.114.114.114
```
### 增加了开机启动
```
[root@test cron]# ll /etc/init.d/wipefs
lrwxrwxrwx 1 root root 11 Nov 22 06:00 /etc/init.d/wipefs -> /bin/wipefs
[root@test rc3.d]# pwd
/etc/rc.d/rc3.d
[root@test rc3.d]# ll -h |grep wipefs
lrwxrwxrwx 1 root root 18 Nov 22 06:00 S01wipefs -> /etc/init.d/wipefs
[root@test rc3.d]# cd /etc/rc3.d/
[root@test rc3.d]# ll -h |grep wipefs
lrwxrwxrwx 1 root root 18 Nov 22 06:00 S01wipefs -> /etc/init.d/wipefs
[root@test init.d]# pwd
/etc/init.d
[root@test init.d]# ll acpidtd
-rwxr-xr-x 1 root root 1223753 Nov 20 16:03 acpidtd
[root@test rc3.d]# ll -h |grep acpidtd
lrwxrwxrwx 1 root root 19 Nov 20 16:03 S01acpidtd -> /etc/init.d/acpidtd
[root@test rc3.d]# pwd
/etc/rc3.d
[root@test rc3.d]# cd /etc/rc.d/rc3.d/
[root@test rc3.d]# ll -h |grep acpidtd
lrwxrwxrwx 1 root root 19 Nov 20 16:03 S01acpidtd -> /etc/init.d/acpidtd
[root@test rc3.d]# ll /bin/ddus-uidgen
-rwxr-xr-x 1 root root 1223753 Nov 20 16:03 /bin/ddus-uidgen
[root@test rc3.d]# ll /etc/resolv.conf
-rw-r--r-- 1 root root 106 Nov 22 06:00 /etc/resolv.conf
```
### 清理
```
pkill wipefs
echo "nameserver 114.114.114.114" > /etc/resolv.conf
chattr -i /bin/wipefs
chattr -i /bin/ddus-uidgen
chattr -i /etc/init.d/acpidtd
rm -rf /bin/wipefs
rm -rf /etc/init.d/wipefs
rm -rf /bin/ddus-uidgen
rm -rf /etc/init.d/acpidtd
rm -rf /etc/rc0.d/S01wipefs
rm -rf /etc/rc1.d/S01wipefs
rm -rf /etc/rc2.d/S01wipefs
rm -rf /etc/rc3.d/S01wipefs
rm -rf /etc/rc4.d/S01wipefs
rm -rf /etc/rc5.d/S01wipefs
rm -rf /etc/rc6.d/S01wipefs
rm -rf /etc/rc.d/rc0.d/S01wipefs
rm -rf /etc/rc.d/rc1.d/S01wipefs
rm -rf /etc/rc.d/rc2.d/S01wipefs
rm -rf /etc/rc.d/rc3.d/S01wipefs
rm -rf /etc/rc.d/rc4.d/S01wipefs
rm -rf /etc/rc.d/rc5.d/S01wipefs
rm -rf /etc/rc.d/rc6.d/S01wipefs
rm -rf /etc/rc0.d/acpidtd
rm -rf /etc/rc1.d/acpidtd
rm -rf /etc/rc2.d/acpidtd
rm -rf /etc/rc3.d/acpidtd
rm -rf /etc/rc4.d/acpidtd
rm -rf /etc/rc5.d/acpidtd
rm -rf /etc/rc6.d/acpidtd
rm -rf /etc/rc.d/rc0.d/acpidtd
rm -rf /etc/rc.d/rc1.d/acpidtd
rm -rf /etc/rc.d/rc2.d/acpidtd
rm -rf /etc/rc.d/rc3.d/acpidtd
rm -rf /etc/rc.d/rc4.d/acpidtd
rm -rf /etc/rc.d/rc5.d/acpidtd
rm -rf /etc/rc.d/rc6.d/acpidtd
```
