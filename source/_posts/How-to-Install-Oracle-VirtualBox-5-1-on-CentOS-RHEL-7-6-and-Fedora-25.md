---
title: How to Install Oracle VirtualBox 5.1 on CentOS/RHEL 7/6 and Fedora 25
date: 2017-08-08 10:24:22
tags:
categories:
  - 技术

---
Oracle VirtualBox is a cross-platform virtualization application. It installs on your existing Intel or AMD-based computers, whether they are running Windows, Mac, Linux or Solaris operating systems. It extends the capabilities of your existing computer so that it can run multiple operating systems at the same time. Click here to read more about VirtualBox

Oracle has released VirtualBox 5.1.14 maintenance release of VirtualBox 5.1 on January 17th, 2017. In this release VirtualBox has improves stability and fixes regressions. To read more about this release read changelog.

This article will help you to install Oracle VirtualBox 5.1 on CentOS, Redhat and Fedora systems using Yum.

Step 1 – Add Required Yum Repositories
===============
Firstly you are required to add VirtualBox yum repository in your system. Download repository file from its official site and place it under at __ /etc/yum.repos.d/virtualbox.repo __ .First navigate to **/etc/yum.repos.d/** directory and use one of below commands as per your operating system.
```
# cd /etc/yum.repos.d/

For CentOS/RHEL Systems:
# wget http://download.virtualbox.org/virtualbox/rpm/rhel/virtualbox.repo

For Fedora Systems:
# wget http://download.virtualbox.org/virtualbox/rpm/fedora/virtualbox.repo
```
__ CentOS/RHEL __ Users also need to add EPEL yum repository using one of the following commands.
```
CentOS/RHEL 7, 64 Bit (x86_64):
# rpm -Uvh http://epel.mirror.net.in/epel/7/x86_64/e/epel-release-7-9.noarch.rpm

CentOS/RHEL 6, 64 Bit (x86_64):
# rpm -Uvh http://epel.mirror.net.in/epel/6/x86_64/epel-release-6-8.noarch.rpm

```
Step 2 – Install Required Packages
==========
Before installing VirtualBox make sure to install all required packages to run VirtualBox like kernel-headers, kernel-devels etc. Use the following command to install required packages.

```
# yum install gcc make patch  dkms qt libgomp
# yum install kernel-headers kernel-devel fontforge binutils glibc-headers glibc-devel
```

Step 3 – Setup Environment Variable
===============
VirtualBox installation required kernel source code to install required modules, So we need to configure environment variable __ KERN_DIR __ to which VirtualBox get kernel source code. In my case latest kernel source is available in ** 2.6.32-504.3.3.el6.x86_64 ** directory under ** /usr/src/kernels/ **. Make sure you are using correct source path.
```
# export KERN_DIR=/usr/src/kernels/2.6.32-504.3.3.el6.x86_64
```

Step 4 – Install Oracle VirtualBox and Setup
================
Use the following command to install VirtualBox 5.1 using yum command line tool. It will install the latest version of VirtualBox 5.1.x on your system.
```
# yum install VirtualBox-5.1
```

After installation, we need to rebuild kernel modules using the following command.
```
sudo /usr/lib/virtualbox/vboxdrv.sh setup
```

Step 5 – Start VirtualBox
================
Use following command to start VirtualBox from X windows. You can switch to GUI mode using ** init 5 ** or ** startx ** commands from terminal.

```
# virtualbox &
```
