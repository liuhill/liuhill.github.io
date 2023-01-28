---
title: How to resize a VirtualBox vmdk file
date: 2017-08-01 14:48:57
tags:
categories:
  - 技术

---

548
down vote
yes, Brian, you are right: those are the steps, but if you want to end having back a vmdk hard disk (maybe you are interested in using the disk in vwmare too) you miss one more step. So the complete howto is:
```
VBoxManage clonehd "source.vmdk" "cloned.vdi" --format vdi
VBoxManage modifyhd "cloned.vdi" --resize 51200
VBoxManage clonehd "cloned.vdi" "resized.vmdk" --format vmdk
The above will resize the hard disk up to 50GB (50 * 1024MB).
```
To complete things you need to resize the drive too! To achieve this, you might want to download gparted iso and boot from that iso to resize your drive (select the iso from within the virtualbox settings).

P.S. If your new size is too small, you'll get the same error even with your new vdi file.
