---
title: imaxabsConfigure HUAWEI E1750 HSPA USB Stick in Linux → Configure USB modem HUAWEI E3131 in Linux
date: 2017-03-25 13:24:18
tags:
---
For installing device HUAWEI E3131 in CentOS 6.4 Linux you have to follow instructions below. Can be used to send SMS by AT command and also to establish HiSpeed Mobile Internet connection.




##Install requirements

You have to download usb_modeswitch and data with configurations.

```
[root@localhost ~]$ wget http://www.draisberghof.de/usb_modeswitch/usb-modeswitch-1.2.7.tar.bz2
[root@localhost ~]$ wget http://www.draisberghof.de/usb_modeswitch/usb-modeswitch-data-20130807.tar.bz2
[root@localhost ~]$ yum install libusb libusb-devel minicom wvdial
[root@localhost ~]$ bzip2 -d usb-modeswitch-1.2.7.tar.bz2
[root@localhost ~]$ bzip2 -d usb-modeswitch-data-20130807.tar.bz2
[root@localhost ~]$ tar -xf usb-modeswitch-1.2.7.tar
[root@localhost ~]$ tar -xf usb-modeswitch-data-20130807.tar
[root@localhost ~]$ cd usb-modeswitch-1.2.7
[root@localhost usb-modeswitch-1.2.7]$ make all
cc -o usb_modeswitch usb_modeswitch.c -Wall -l usb
sed 's_!/usr/bin/tclsh_!'"/usr/bin/jimsh"'_' < usb_modeswitch.tcl > usb_modeswitch_dispatcher
[root@localhost usb-modeswitch-1.2.7]$ make install
sed 's_!/usr/bin/tclsh_!'"/usr/bin/jimsh"'_' < usb_modeswitch.tcl > usb_modeswitch_dispatcher
install -D --mode=755 usb_modeswitch /usr/sbin/usb_modeswitch
install -D --mode=755 usb_modeswitch.sh /lib/udev/usb_modeswitch
install -D --mode=644 usb_modeswitch.conf /etc/usb_modeswitch.conf
install -D --mode=644 usb_modeswitch.1 /usr/share/man/man1/usb_modeswitch.1
install -D --mode=755 usb_modeswitch_dispatcher /usr/sbin/usb_modeswitch_dispatcher
install -d /var/lib/usb_modeswitch
[root@localhost usb-modeswitch-1.2.7]$ cd ..
[root@localhost ~]$ cd usb-modeswitch-data-20130807
[root@localhost usb-modeswitch-data-20130807]$ make install
install -d /usr/share/usb_modeswitch
install -d /etc/usb_modeswitch.d
install -D --mode=644 40-usb_modeswitch.rules /lib/udev/rules.d/40-usb_modeswitch.rules
install --mode=644 -t /usr/share/usb_modeswitch ./usb_modeswitch.d/*
custom logging function 0x7fd5344c7010 registered
selinux=0
calling: control

```

## Manual configuration
Now we have to install modules and configure device to modem mode.
```
[root@localhost ~]$ lsusb
Bus 001 Device 029: ID 12d1:14fe Huawei Technologies Co., Ltd.
[root@localhost ~]$ modprobe option usbserial usb_wwan
[root@localhost ~]$ /usr/sbin/usb_modeswitch -H -v 12d1 -p 14fe -c /etc/usb_modeswitch.d/12d1\:14fe
[root@localhost ~]$ lsusb
Bus 001 Device 030: ID 12d1:1506 Huawei Technologies Co., Ltd. E398 LTE/UMTS/GSM Modem/Networkcard
[root@localhost ~]$ ls -la /dev/ttyUSB*
crw-rw---- 1 root dialout 188,  0 23. srp 08.49 /dev/ttyUSB0
crw-rw---- 1 root dialout 188,  1 23. srp 08.49 /dev/ttyUSB1
crw-rw---- 1 root dialout 188,  2 23. srp 08.49 /dev/ttyUSB2
crw-rw---- 1 root dialout 188,  3 23. srp 08.49 /dev/ttyUSB3
```

## Testing
Now we have 4 USB serial ports then we can use minicom -s -c on command to test send SMS by
AT commands. For dialup internet you have to use wvdial.

```
Now we have 4 USB serial ports then we can use minicom -s -c on command to test send SMS by
AT commands. For dialup internet you have to use wvdial.
```

```
[Dialer Defaults]
Init1 = AT+CGDCONT=1,"IP","internet.t-mobile.cz"
Init2 =
Init3 =
Stupid Mode = 1
Ask Password = 0
Phone = *99***1#
Modem = /dev/ttyUSB0
Username = neco
Password = neco
Baud = 9600
Auto DNS = yes
Dial Command = ATDT

```

> ystem: CentOS 6.4 x86_64
> GIMP: 2.6 x86_64
> Inkscape: 0.47 x86_64
> GNOME: 2.28.2 x86_64
