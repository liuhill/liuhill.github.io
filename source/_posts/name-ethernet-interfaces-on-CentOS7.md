---
title: name ethernet interfaces on CentOS7
date: 2017-07-27 15:14:53
tags:
---
I've learned how to continue to use the ethX prefix. I'm still using the udev rules that I mentioned in post above. So at bootup udev will rename my interfaces netX. And then using /etc/rc.local I've included the following commands to rename them from netX back to ethX.

```
/sbin/ip link set net0 down
/sbin/ip link set net0 name eth0
/sbin/ip link set eth0 up
...
```

Using the ip command I can change the name of the interfaces after the initial bootup is completed.
