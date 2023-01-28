---
title: Docker client is newer than server
date: 2016-09-12 16:20:54
tags:
  - docker
categories:
  - 技术

---
###Docker client is newer than server
执行`docker pull voduytuan/jenkins-php-docker`时出错

```
Using default tag: latest
Warning: failed to get default registry endpoint from daemon (Error response from daemon: client is newer than server (client API version: 1.24, server API version: 1.23)). Using system default: https://index.docker.io/v1/
Error response from daemon: client is newer than server (client API version: 1.24, server API version: 1.23)

```

执行命令
`export DOCKER_API_VERSION=1.23 ` solved my problem. 
