---
title: CentOS7 Command line install VirtuBox
date: 2017-08-03 20:24:12
tags:
        - centos
        - 命令行
        - 虚拟机
categories:
  - 技术

---
## 1.导出OVA -> test.ova

## 2.copy到服务器&导入
```
[root@localhost vm] BoxManage import test.ovf
```


## 查看虚拟机镜像文件配置信息
```
[root@localhost vm]# VBoxManage showvminfo HKAirline
Name:            HKAirline
Groups:          /
Guest OS:        Red Hat (64-bit)
UUID:            041f669d-2974-4322-a046-a55903285424
Config file:     /root/VirtualBox VMs/HKAirline/HKAirline.vbox
Snapshot folder: /root/VirtualBox VMs/HKAirline/Snapshots
Log folder:      /root/VirtualBox VMs/HKAirline/Logs
Hardware UUID:   041f669d-2974-4322-a046-a55903285424
Memory size:     2048MB
Page Fusion:     off
VRAM size:       16MB
CPU exec cap:    100%
HPET:            off
Chipset:         piix3
Firmware:        BIOS
Number of CPUs:  1
PAE:             on
Long Mode:       on
Triple Fault Reset: off
APIC:            on
X2APIC:          on
CPUID Portability Level: 0
CPUID overrides: None
Boot menu mode:  message and menu
Boot Device (1): Floppy
Boot Device (2): DVD
Boot Device (3): HardDisk
Boot Device (4): Not Assigned
ACPI:            on
IOAPIC:          on
BIOS APIC mode:  APIC
Time offset:     0ms
RTC:             UTC
Hardw. virt.ext: on
Nested Paging:   on
Large Pages:     on
VT-x VPID:       on
VT-x unr. exec.: on
Paravirt. Provider: Default
Effective Paravirt. Provider: KVM
State:           powered off (since 2017-08-03T11:27:45.000000000)
Monitor count:   1
3D Acceleration: off
2D Video Acceleration: off
Teleporter Enabled: off
Teleporter Port: 0
Teleporter Address:
Teleporter Password:
Tracing Enabled: off
Allow Tracing to Access VM: off
Tracing Configuration:
Autostart Enabled: off
Autostart Delay: 0
Default Frontend:
Storage Controller Name (0):            IDE
Storage Controller Type (0):            PIIX4
Storage Controller Instance Number (0): 0
Storage Controller Max Port Count (0):  2
Storage Controller Port Count (0):      2
Storage Controller Bootable (0):        on
Storage Controller Name (1):            SATA
Storage Controller Type (1):            IntelAhci
Storage Controller Instance Number (1): 0
Storage Controller Max Port Count (1):  30
Storage Controller Port Count (1):      2
Storage Controller Bootable (1):        on
IDE (1, 0): Empty
SATA (0, 0): /root/VirtualBox VMs/HKAirline/HKAirline-disk001.vmdk (UUID: 6afa88f3-3035-4aa5-a940-f6ec43bc56bb)
NIC 1:           MAC: 0800274D5932, Attachment: Bridged Interface 'eth0', Cable connected: on, Trace: off (file: none), Type: 82540EM, Reported speed: 0 Mbps, Boot priority: 0, Promisc Policy: deny, Bandwidth group: none
NIC 2:           MAC: 0800279B57E7, Attachment: Bridged Interface 'eth0', Cable connected: on, Trace: off (file: none), Type: 82540EM, Reported speed: 0 Mbps, Boot priority: 0, Promisc Policy: deny, Bandwidth group: none
NIC 3:           MAC: 080027716E47, Attachment: Bridged Interface 'eth0', Cable connected: on, Trace: off (file: none), Type: 82540EM, Reported speed: 0 Mbps, Boot priority: 0, Promisc Policy: deny, Bandwidth group: none
NIC 4:           MAC: 080027C363BB, Attachment: Bridged Interface 'eth0', Cable connected: on, Trace: off (file: none), Type: 82540EM, Reported speed: 0 Mbps, Boot priority: 0, Promisc Policy: deny, Bandwidth group: none
NIC 5:           disabled
NIC 6:           disabled
NIC 7:           disabled
NIC 8:           disabled
Pointing Device: PS/2 Mouse
Keyboard Device: PS/2 Keyboard
UART 1:          disabled
UART 2:          disabled
UART 3:          disabled
UART 4:          disabled
LPT 1:           disabled
LPT 2:           disabled
Audio:           enabled (Driver: ALSA, Controller: AC97, Codec: AD1980)
Clipboard Mode:  disabled
Drag and drop Mode: disabled
VRDE:            enabled (Address 0.0.0.0, Ports 3389, MultiConn: off, ReuseSingleConn: off, Authentication type: external)
Video redirection: disabled
VRDE property: TCP/Ports  = "3389"
VRDE property: TCP/Address = <not set>
VRDE property: VideoChannel/Enabled = <not set>
VRDE property: VideoChannel/Quality = <not set>
VRDE property: VideoChannel/DownscaleProtection = <not set>
VRDE property: Client/DisableDisplay = <not set>
VRDE property: Client/DisableInput = <not set>
VRDE property: Client/DisableAudio = <not set>
VRDE property: Client/DisableUSB = <not set>
VRDE property: Client/DisableClipboard = <not set>
VRDE property: Client/DisableUpstreamAudio = <not set>
VRDE property: Client/DisableRDPDR = <not set>
VRDE property: H3DRedirect/Enabled = <not set>
VRDE property: Security/Method = <not set>
VRDE property: Security/ServerCertificate = <not set>
VRDE property: Security/ServerPrivateKey = <not set>
VRDE property: Security/CACertificate = <not set>
VRDE property: Audio/RateCorrectionMode = <not set>
VRDE property: Audio/LogPath = <not set>
USB:             enabled
EHCI:            disabled
XHCI:            disabled

USB Device Filters:

<none>

Bandwidth groups:  <none>

Shared folders:  <none>

Video capturing:    not active
Capture screens:    0
Capture file:       /root/VirtualBox VMs/HKAirline/HKAirline.webm
Capture dimensions: 1024x768
Capture rate:       512 kbps
Capture FPS:        25

Guest:

Configured memory balloon size:      0 MB

```

## 3.检查服务器的网卡名称
```
[root@localhost var]# ifconfig
enp3s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 11.0.0.30  netmask 255.255.255.0  broadcast 11.0.0.255
        inet6 fe80::7627:eaff:fe0b:cbdd  prefixlen 64  scopeid 0x20<link>
        ether 74:27:ea:0b:cb:dd  txqueuelen 1000  (Ethernet)
        RX packets 1176518  bytes 1473593219 (1.3 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 629933  bytes 45138166 (43.0 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 0  (Local Loopback)
        RX packets 83  bytes 7928 (7.7 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 83  bytes 7928 (7.7 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


```


## 5. 启动网卡配置
```
/usr/lib/virtualbox/vboxdrv.sh setup
```
### 5.1 安装内核
```
[root@localhost ~]# yum install kernel-headers-$(uname -r)
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.bit.edu.cn
 * epel: mirrors.tuna.tsinghua.edu.cn
 * extras: mirrors.btte.net
 * updates: mirror.bit.edu.cn
No package kernel-headers-3.10.0-327.el7.x86_64 available.
Error: Nothing to do
```

### 5.2 查看内核
```
[root@localhost ~]# uname -a
Linux localhost.localdomain 3.10.0-327.el7.x86_64 #1 SMP Thu Nov 19 22:10:57 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux

```
### 5.3 下载匹配内核
```
[root@localhost ~]# wget ftp://ftp.pbone.net/mirror/ftp.scientificlinux.org/linux/scientific/7.0/x86_64/updates/security/kernel-devel-3.10.0-
327.el7.x86_64.rpm
--2017-08-03 09:00:48--  ftp://ftp.pbone.net/mirror/ftp.scientificlinux.org/linux/scientific/7.0/x86_64/updates/security/kernel-devel-3.10.0-327.el7.x86_64.rpm
           => ‘kernel-devel-3.10.0-327.el7.x86_64.rpm.1’
Resolving ftp.pbone.net (ftp.pbone.net)... 85.14.85.4
Connecting to ftp.pbone.net (ftp.pbone.net)|85.14.85.4|:21... connected.
Logging in as anonymous ... Logged in!
==> SYST ... done.    ==> PWD ... done.
==> TYPE I ... done.  ==> CWD (1) /mirror/ftp.scientificlinux.org/linux/scientific/7.0/x86_64/updates/security ... done.
==> SIZE kernel-devel-3.10.0-327.el7.x86_64.rpm ... 11470260
==> PASV ... done.    ==> RETR kernel-devel-3.10.0-327.el7.x86_64.rpm ... done.
Length: 11470260 (11M) (unauthoritative)

100%[====================================================================================================>] 11,470,260   603KB/s   in 28s    

2017-08-03 09:01:21 (406 KB/s) - ‘kernel-devel-3.10.0-327.el7.x86_64.rpm’ saved [11470260]


```
#### 安装依赖包
```
yum install make gcc libpcap libpcap-devel -y
```

### 安装
通过如下命令直接安装：
```
rpm -ivh kernel-devel-3.10.0-327.el7.x86_64.rpm
```
如果系统已经安装了较高版本的内核头文件，则需要通过如下命令实现降级：
```
rpm -Uvh --oldpackage kernel-devel-3.10.0-327.el7.x86_64.rpm
```

## 查看Bradge接口
```
[root@localhost vm]# VBoxManage list bridgedifs
Name:            enp3s0
GUID:            33706e65-3073-4000-8000-7427ea0bcbdd
DHCP:            Disabled
IPAddress:       11.0.0.30
NetworkMask:     255.255.255.0
IPV6Address:     fe80:0000:0000:0000:7627:eaff:fe0b:cbdd
IPV6NetworkMaskPrefixLength: 64
HardwareAddress: 74:27:ea:0b:cb:dd
MediumType:      Ethernet
Status:          Up
VBoxNetworkName: HostInterfaceNetworking-enp3s0

```

### 修改网卡设备的名称
```
[root@localhost vm]# VBoxManage modifyvm HKAirline --bridgeadapter4 enp3s0
```



### 启动虚拟机
```
VBoxManage startvm HKAirline --type headless
```
