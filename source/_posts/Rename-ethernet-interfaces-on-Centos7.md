---
title: Rename ethernet interfaces on Centos7
date: 2017-07-31 11:17:33
tags:
    - 网络
    - 命令行
    - centos 7
categories:
  - 技术
---
### 0 Prepare
      yum install pciutils -y


### 1 get PCI info:
```
[root@localhost ~] lspci | grep Ethernet   
05:00.0 Ethernet controller: Intel Corporation 82574L Gigabit Network Connection                                     
06:00.0 Ethernet controller: Intel Corporation 82574L Gigabit Network Connection                                     
07:00.0 Ethernet controller: Intel Corporation 82574L Gigabit Network Connection                                     
08:00.0 Ethernet controller: Intel Corporation 82574L Gigabit Network Connection

```

### 2 add config to `cat /usr/lib/udev/rules.d/60-net.rules `.
***Notice:*** param `KERNELS`

```
[root@localhost ~]# cat /usr/lib/udev/rules.d/60-net.rules
ACTION=="add", SUBSYSTEM=="net", DRIVERS=="?*", ATTR{type}=="1", PROGRAM="/lib/udev/rename_device", RESULT=="?*", NAME="$result"
SUBSYSTEMS=="pci", ACTION=="add", DRIVERS=="?*", KERNELS=="0000:00:05.0", NAME="vfnet0"
SUBSYSTEMS=="pci", ACTION=="add", DRIVERS=="?*", KERNELS=="0000:00:06.0", NAME="vfnet1"
SUBSYSTEMS=="pci", ACTION=="add", DRIVERS=="?*", KERNELS=="0000:00:07.0", NAME="vfnet2"
SUBSYSTEMS=="pci", ACTION=="add", DRIVERS=="?*", KERNELS=="0000:00:08.0", NAME="vfnet3"
```

### 3 reboot

### 4 check
```
[root@localhost ~]# cat /usr/lib/udev/rules.d/60-net.rules
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: vfnet0: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master bond0 state UP qlen 1000
    link/ether f2:d5:5d:21:4e:38 brd ff:ff:ff:ff:ff:ff
3: vfnet1: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master bond0 state UP qlen 1000
    link/ether 3e:ec:21:1a:d2:86 brd ff:ff:ff:ff:ff:ff
4: vfnet2: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master bond0 state UP qlen 1000
    link/ether be:70:05:01:46:04 brd ff:ff:ff:ff:ff:ff
5: vfnet3: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast master bond0 state UP qlen 1000
    link/ether e6:de:61:c9:a8:d1 brd ff:ff:ff:ff:ff:ff
```
