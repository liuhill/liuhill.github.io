---
title: 'fsck error on boot: /dev/sda6: UNEXPECTED INCONSISTENCY; RUN fsck MANUALLY'
date: 2018-01-03 12:06:12
tags:
categories:
  - 技术

---
I have noticed that even if you do a fsck on the disk the problem may occur again in a few days.

I have found that the problem is worse on SSD disks than the regular HDD disks. I have found some steps that may fix the problem temporarily.

    fsck -fy /dev/sda1
if sda1 is the right partition - the prompt will tell you exactly which one requires fsck.

After that if the systems boots up you may have another problem with the package management system, so if you open a terminal and type sudo apt-get update you may get an error. Do not worry. Run these commands:
```
sudo apt-get update
sudo apt-get clean
sudo apt-get update
sudo apt-get upgrade
```
My opinion is that there is serious problem in Ubuntu with regard to SSD disks. The community should fix it.

I have found a possible cause of this problem: Probably the system did not shutdown normally.
