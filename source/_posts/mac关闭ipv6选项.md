---
title: mac关闭ipv6选项
date: 2017-07-18 17:43:42
tags:
---
```
$ networksetup -listallnetworkservices  
An asterisk (*) denotes that a network service is disabled.  
SAMSUNG Modem  
Bluetooth DUN  
Thunderbolt Ethernet  
Wi-Fi  
Bluetooth PAN  
Thunderbolt Bridge  
$ networksetup -setv6off Wi-Fi  
```
