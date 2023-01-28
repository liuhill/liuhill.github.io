---
title: How to remove old Docker containers
date: 2017-03-10 10:08:34
tags:
categories:
  - 技术

---

### 1、remove `exit` containers
Until that command is available, you can string docker commands together with other unix commands to get what you need. Here is an example on how to clean up old containers that are weeks old.

    $ docker ps --filter "status=exited" | grep 'weeks ago' | awk '{print $1}' | xargs --no-run-if-empty docker rm

### 2、remove images
    $ docker images | grep "<none>" | awk '{print $3}' | xargs docker rmi

### 3 rm all `stopped` containers
If you want to use awk for this consider this command if you want do rm all stopped containers (without a error because of the first line):

    $ docker ps -a | awk 'NR > 1 {print $1}' | xargs docker rm
