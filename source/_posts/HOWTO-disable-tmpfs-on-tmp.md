---
title: HOWTO disable tmpfs on /tmp?
date: 2017-08-02 10:21:16
tags:
categories:
  - 技术

---
```
$ cat /etc/tmpfiles.d/tmp.conf
#  This file is part of systemd.
#
#  systemd is free software; you can redistribute it and/or modify it
#  under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation; either version 2.1 of the License, or
#  (at your option) any later version.

# See tmpfiles.d(5) for details

# Clear tmp directories separately, to make them easier to override
D /tmp 1777 root root 0
D /var/tmp 1777 root root 10d

# Exclude namespace mountpoints created with PrivateTmp=yes
x /tmp/systemd-private-*
x /var/tmp/systemd-private-*
X /tmp/systemd-private-*/tmp
X /var/tmp/systemd-private-*/tmp
```
