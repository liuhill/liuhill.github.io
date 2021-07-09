---
title: How to setup tagged VLANs on Mac OS X
date: 2017-07-04 14:27:14
tags:
---
1. From the Apple menu (upper left corner of your desktop) choose System Preferences...
- Open Network
- In the toolbar below all of your network interfaces, click the gear wheel to access a dropdown menu
- From that dropdown menu, select Manage Virtual Interfaces...
- Click on + to access another dropdown menu
- From that dropdown menu, select New VLAN...
- Enter an appropriate name for your new VLAN, e.g. DMZ
- Enter the desired tag, e.g. 23
- Choose the correct physical interface, normally Ethernet
- Click Create
- Click Done
- Configure your new VLAN interface as you would configure a physical interface, e.g. assign it an IP address, network mask, etc.
- Click Apply
- Close the network preferences window.
