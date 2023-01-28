---
title: Nslookup works but ping fails
date: 2022-03-22 11:14:08
tags:
  - 命令行
  - 网络
categories:
  - 技术

---
### QUESTION 
On my Windows XP workstation, I can find the machine I want to connect to in DNS with nslookup:
```
nslookup wolfman
Server: dns.company.com
Address: 192.168.1.38

Name: wolfman.company.com
Address: 192.168.1.178
```
On my Windows XP workstation, I can find the machine I want to connect to in DNS with nslookup:
```
nslookup wolfman
Server: dns.company.com
Address: 192.168.1.38
Name: wolfman.company.com
Address: 192.168.1.178
```
But, when I try to connect to that machine, I get an error telling me that the machine can't be found (i.e., can't be looked up in DNS):
```
C:\> ping wolfman
Ping request could not find host wolfman. Please check the name and try again.
```

I am able to connect if I use the IP address directly:
```
C:\> ping 192.168.1.178

Pinging 192.168.1.178 with 32 bytes of data:

Reply from 192.168.1.178: bytes=32 time=41ms TTL=126
Reply from 192.168.1.178: bytes=32 time=41ms TTL=126
Reply from 192.168.1.178: bytes=32 time=44ms TTL=126
Reply from 192.168.1.178: bytes=32 time=38ms TTL=126
```

### ANSWER 1
I believe that nslookup opens a winsock connection on the DNS port and issues a query, whereas ping uses the DNS Client service. You could try and stop this service and see whether this makes a difference.

Some commands that will reinitialize various network states :

- Reset WINSOCK entries to installation defaults : `netsh winsock reset catalog`
- Reset TCP/IP stack to installation defaults : `netsh int ip reset reset.log`
- Flush DNS resolver cache : `ipconfig /flushdns`
- Renew DNS client registration and refresh DHCP leases : `ipconfig /registerdns`
- Flush routing table : `route /f` (this will remove all your gateways until you restart!)

### ANSWER 2
Try ping with hostname followed by a dot. So instead of `ping wolfman` use `ping wolfman.`

That should get you resolving without having to do workarounds with hosts file, etc.
