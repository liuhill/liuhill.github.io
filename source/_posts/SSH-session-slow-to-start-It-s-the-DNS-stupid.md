---
title: SSH session slow to start? It's the DNS stupid~~
date: 2017-07-11 11:23:09
tags:
---
Ever tried logging into a machine with ssh and found you have to wait much longer than reasonable for the session to start? This happened to me a few times and was especially annoying with machines on my local network (or a VM attached to a virtual network) that should be letting me in immediately.

I eventually got mad enough to strace the SSH daemon and debug what was going on and it turns out it's a DNS thing. Basically the session is slow to start because the SSH server is trying to lookup the hostname of the SSH client and for whatever reason it's timing out (e.g., it can't reach a nameserver, because you happen to be offline)

There are a couple of very simple ways to fix that:
```
add "UseDNS no" to /etc/ssh/sshd_config
add the client's net address to the server's /etc/hosts
```

*** 改项可能会导致远程连接失败
