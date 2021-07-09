---
title: 'CentOS / RHEL: Remove Routes 169.254.0.0 / 255.255.0.0 From the System'
date: 2017-08-08 18:52:17
tags:
---
zeroconf (Zero configuration networking), is a techniques that automatically creates a usable Internet Protocol (IP) network without manual operator intervention or special configuration servers. 169.254.0.0/255.255.0.0 route is part of zeroconf under RHEL 6 / CentOS 6 or older versions. To see current routing table, enter:
```
# route -n
```
Sample outputs:

```
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
74.8x.4y.zz     0.0.0.0         255.255.255.248 U     0      0        0 eth1
10.10.29.64     0.0.0.0         255.255.255.192 U     0      0        0 eth0
169.254.0.0     0.0.0.0         255.255.0.0     U     1002   0        0 eth0
169.254.0.0     0.0.0.0         255.255.0.0     U     1003   0        0 eth1
10.0.0.0        10.10.29.65     255.0.0.0       UG    0      0        0 eth0
0.0.0.0         74.8x.yy.zz     0.0.0.0         UG    0      0        0 eth1
```

Every time the server or Linux desktop boots, the zeroconf route 169.254.0.0 is enabled and added to the kernel routing table. To disable zeroconf route under RHEL / CentOS / Fedora Linux, enter:

```
# vi /etc/sysconfig/network
```
Append the following directive:

NOZEROCONF=yes
Save and close the file. Reboot the system / server or restart the networking service:
# /etc/init.d/network restart

Verify routing table, enter:
# route -n

OR
# ip route
