---
title: 'Docker - dial tcp: lookup index.docker.io: no such host - Solution'
id: 378
categories:
  - 技术
date: 2016-06-28 17:04:17
tags:
    - docker
---

## 1. Symptoms

While using Docker a user is unable to perform tasks such as pull new image or search for new images while the following error message appears:

     # docker pull debian:8
     Pulling repository debian
     FATA[0053] Get https://index.docker.io/v1/repositories/library/debian/images: dial tcp: lookup index.docker.io: no such host


## 2. Solution

While this is a randomly occurring error it is frequently linked to a DNS resolution query by docker services. First, make sure you are running up to date docker installation. If this does not resolve your issue then try to resolve this error by restarting your docker service:

    # service docker restart
    Redirecting to /bin/systemctl restart  docker.service


In case that you are unable to restart your docker service at a present time you my try to temporarily resolve this issue by including the IP address of the host in question eg. `index.docker.io` to your `/etc/hosts` file. Resolve the IP address:

    # dig index.docker.io +noall +answer +nocomments

    ; <<>> DiG 9.9.4-P2-RedHat-9.9.4-18.P2.fc20 <<>> index.docker.io +noall +answer +nocomments
    ;; global options: +cmd
    index.docker.io.        30      IN      A       162.242.195.84


Next, include the IP address to your system's `/etc/hosts` file:

    # echo '162.242.195.84 index.docker.io' >> /etc/hosts
