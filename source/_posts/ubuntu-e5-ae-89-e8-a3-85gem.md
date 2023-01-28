---
title: ubuntu安装gem grunt-contrib-sass
id: 215
categories:
  - 技术
date: 2015-08-09 19:23:54
tags:
  - ubuntu
  - 命令行
---

    # Install gem sass for grunt-contrib-sass
    RUN apt-get update -qq &amp;&amp; apt-get install -y build-essential
    RUN apt-get install -y ruby
    RUN gem install sass
    
