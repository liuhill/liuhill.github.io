---
title: (转)LNMP整合安装Redmine2.3实录
tags:
  - nginx
  - redmine
id: 50
categories:
  - server
date: 2014-11-07 18:23:00
---

自上一次在LNMP环境下安装过Redmine之后发表了《LNMP下安装Redmine2.3手记》，Inhu决定再一次尝试。因为上一次Inhu是通过折中的办法，也就是利用bitnami+lnmp这种做法实现的。现在我再一次决定在LNMP环境下不利用任何的一键安装包安装Redmine。

首先，我在这里不得不声明几点。

服务器系统时Centos6，试过在Centos5下安装，但由于软件库等各种问题最后在安装ImageMagick的时候失败了。所以建议大家使用Centos6。

首先，我们都安装好了Lnmp（一键安装，没修改任何配置目录的情况下）后。开始实施我们的Redmine安装了。

### 1 安装ruby环境

执行以下命令：

    yum -y install zlib-devel curl-devel openssl-devel apr-devel apr-util-devel mysql-devel ImageMagick ImageMagick-devel rdoc gcc-c++ ruby ruby-devel 
    

    上面的命令是安装各种要用到的软件包，这时候Ruby应该是装好的了，你可以通过命令:

    ruby –v 
    

    进行查看Ruby的版本。当安装完以后我们实行第二步。

    ### 2 安装 RubyGems

    访问：http://rubygems.org/pages/download

    然后下载zip包或者tar包，然后解压出来后，进入目录执行以下命令：

    ruby setup.rb gem -v gem install passenger
    

    如果是国内主机的话，建议使用

    http://ruby.taobao.org/
    

    淘宝提供的一个RubyGems源。如何使用网站上面有详细说明，如果是国外主机就无需设置了。 然后执行以下命令：

    passenger-install-nginx-module
    

    这时候我们的操作步骤应该是： 填入lnmp目录下的Nginx源目录.例如:

    > /root/download/lnmp1.0-full/nginx-1.2.7
    > /usr/local/nginx 
    

    如果需要IPV6的话，在设置配置参数的时候加上 –with-ipv6 然后猛的回车，看到一大堆的编译安装、编译安装了，如无意外就安装成功了。然后它会高亮提示你如何设置Nginx。

    http {
           ...
            passenger_root /usr/lib/ruby/gems/1.8/gems/passenger-4.0.5; 
            passenger_ruby /usr/bin/ruby; ... 
         }
    

    到这里，Web的容器环境已经配好了。 然后我们把下载好的Redmine解压出来，放到 /home/www/ 下。

    然后进入config目录，复制修改 database.yml.example 文件。

    cd /home/www/redmine/config cp database.yml.example database.yml
    vi database.yml
    

    修改这个database.yml的时候我们在修改DBname、账户、密码外还要注意的是我们要加一句，如下面的例子：

    production:
    adapter: mysql2
    database: redmine
    host: localhost
    username: -u
    password: “-p”
    encoding: utf8
    socket: /tmp/mysql.sock
    

    除了production外，我都用 “#”注释掉了。因为用不着。然后去创建数据库了，这里不多说。
    然后我们返回上一级目录，修改GemFile。

    cd ..
    vi Gemfile
    

    在Gemfile第二行开始添加以下内容(可以不添加)：

    gem "rake", "10.0.4"
    gem "rack", "1.4.5"
    gem "rubytree", "0.8.3", :require => "tree"
    gem "RedCloth", "~>4.2.9", :require => "redcloth" # for CodeRay    
    gem "mysql"
    

    添加完之后，执行：

    gem install bundle
    bundle install --without development test
    

    经过一轮等待后，可以看到成功的界面了吗？没看到，遇到问题了？慢慢搜索解决吧。哈哈
    等等……Redmine还没有安装成功呢！
    好了，然后执行以下指令吧：

    rake generate_secret_token
    RAILS_ENV=production rake db:migrate
    RAILS_ENV=production rake redmine:load_default_data
    

    非root用户需要添加用户

    mkdir -p tmp tmp/pdf public/plugin_assets
    sudo chown -R redmine:redmine files log tmp public/plugin_assets
    sudo chmod -R 755 files log tmp public/plugin_assets
    

    启动服务

    ruby script/rails server webrick -e production
    

    很好，这时应该能测试通过了。那么现在就要去配置Nginx了，在lnmp那里创建一个vHost，然后修改vHost配置文件如下：

    server {
             listen 81;
             server_name pm.techoinfo.com;
             index index.html index.htm index.php;
             root /home/www/redmine/public;
             passenger_enabled on;
             access_log  /home/wwwlogs/y.log  y;
        }

恩恩，就这样大功告成了。记住，Root的目录是指向 redmine 下的 public 目录哦，别搞错了！

如果在安装过程有问题，可以留言给我，因为我也遇到过很多问题。各种环境不一样各种问题啊。
原文:http://inhu.net/install-redmine-with-lnmp.html
