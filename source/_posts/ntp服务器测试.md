---
title: ntp服务器测试
date: 2021-07-22 10:15:44
tags: 
    - ntp
    - 时间服务器
categories:
  - 技术

---

## 1. windows 测试
命令行输入以下命令：
```
w32tm /stripchart /computer:ntp_server_address
```
例如：`w32tm /stripchart /computer:time.windows.com`

## 2. linux 测试
```
[root@centos6 ~]# ntpdate -d ntp1.aliyun.com
27 Mar 22:04:38 ntpdate[1049]: ntpdate 4.2.6p5@1.2349-o Wed Dec 19 20:22:35 UTC 2018 (1)
Looking for host ntp1.aliyun.com and service ntp
host found : 120.25.115.20
transmit(120.25.115.20)
receive(120.25.115.20)
transmit(120.25.115.20)
receive(120.25.115.20)
transmit(120.25.115.20)
receive(120.25.115.20)
transmit(120.25.115.20)
receive(120.25.115.20)
server 120.25.115.20, port 123
stratum 2, precision -25, leap 00, trust 000
refid [120.25.115.20], delay 0.07951, dispersion 0.00424
transmitted 4, in filter 4
reference time:    e046016e.965c7e00  Wed, Mar 27 2019 22:04:30.587
originate timestamp: e0460177.12bce60a  Wed, Mar 27 2019 22:04:39.073
transmit timestamp:  e0460177.04f47dc5  Wed, Mar 27 2019 22:04:39.019
filter delay:  0.09312  0.07951  0.08475  0.09390 
         0.00000  0.00000  0.00000  0.00000 
filter offset: 0.004349 0.010768 0.007720 0.019682
         0.000000 0.000000 0.000000 0.000000
delay 0.07951, dispersion 0.00424
offset 0.010768
27 Mar 22:04:39 ntpdate[1049]: adjust time server 120.25.115.20 offset 0.010768 sec
```

## 3. 国内常用时间服务器地址
- 210.72.145.44  (国家授时中心服务器IP地址)
- 133.100.11.8  日本 福冈大学
- time-a.nist.gov 129.6.15.28 NIST, Gaithersburg, Maryland 
- time-b.nist.gov 129.6.15.29 NIST, Gaithersburg, Maryland 
- time-a.timefreq.bldrdoc.gov 132.163.4.101 NIST, Boulder, Colorado 
- time-b.timefreq.bldrdoc.gov 132.163.4.102 NIST, Boulder, Colorado 
- time-c.timefreq.bldrdoc.gov 132.163.4.103 NIST, Boulder, Colorado 
- utcnist.colorado.edu 128.138.140.44 University of Colorado, Boulder 
- time.nist.gov 192.43.244.18 NCAR, Boulder, Colorado 
- time-nw.nist.gov 131.107.1.10 Microsoft, Redmond, Washington 
- nist1.symmetricom.com 69.25.96.13 Symmetricom, San Jose, California 
- nist1-dc.glassey.com 216.200.93.8 Abovenet, Virginia 
- nist1-ny.glassey.com 208.184.49.9 Abovenet, New York City 
- nist1-sj.glassey.com 207.126.98.204 Abovenet, San Jose, California 
- nist1.aol-ca.truetime.com 207.200.81.113 TrueTime, AOL facility, Sunnyvale, California 
- nist1.aol-va.truetime.com 64.236.96.53 TrueTime, AOL facility, Virginia
----
- ntp.sjtu.edu.cn 202.120.2.101 (上海交通大学网络中心NTP服务器地址）
- s1a.time.edu.cn 北京邮电大学
- s1b.time.edu.cn 清华大学
- s1c.time.edu.cn 北京大学
- s1d.time.edu.cn 东南大学
- s1e.time.edu.cn 清华大学
- s2a.time.edu.cn 清华大学
- s2b.time.edu.cn 清华大学
- s2c.time.edu.cn 北京邮电大学
- s2d.time.edu.cn 西南地区网络中心
- s2e.time.edu.cn 西北地区网络中心
- s2f.time.edu.cn 东北地区网络中心
- s2g.time.edu.cn 华东南地区网络中心
- s2h.time.edu.cn 四川大学网络管理中心
- s2j.time.edu.cn 大连理工大学网络中心
- s2k.time.edu.cn CERNET桂林主节点
- s2m.time.edu.cn 北京大学