---
title: >-
  Centos 7: failed to bring up/down networking: configure interface for a trunk
  interface
date: 2017-07-31 12:41:46
tags:
  - centos 
  - 网络
categories:
  - 技术

---
it seems that disabling NetworkManager did the trick :)

```
systemctl stop NetworkManager
systemctl disable NetworkManager
```
