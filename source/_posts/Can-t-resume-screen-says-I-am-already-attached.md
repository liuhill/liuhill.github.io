---
title: 'Can''t resume screen, says I am already attached?'
date: 2017-03-22 16:54:10
tags:
---
he session is still attached on another terminal. The server hasn't detected the network outage on that connection: it only detects the outage when it tries to send a packet and gets an error back or no response after a timeout, but this hasn't happened yet. You're in a common situation where the client detected the outage because it tried to send some input and failed, but the server is just sitting there waiting for input. Eventually the server will send a keepalive packet and detect that the connection is dead.

In the meantime, use the `-d` option to detach the screen session from the terminal where it's in.

```
screen -r -d 30608
screen -rd is pretty much the standard way to attach to an
```
existing screen session.
