title: Redmine2.5.2安装简要笔记
date: 2014-08-08 14:22:52
tags:
    - ruby
---

##一、安装ruby

安装脚本

    #/bin/bash
    curl -L get.rvm.io | bash -s stable
    source ~/.bashrc
    source ~/.bash_profile
    source /usr/local/rvm/scripts/rvm
    sed -i 's!ftp.ruby-lang.org/pub/ruby!ruby.taobao.org/mirrors/ruby!' $rvm_path/config/db
    rvm install 1.9.3
    rvm use 1.9.3 --default
    exit 0

##二、安装redmine

###2.1  下载

略

###2.2 安装

    tar xvf redmine...tar.gz

配置下config目录下的configuration.yml，database.yml，至于mysql的配置省略

    bundle install

完毕后不要忘了执行

    rake db:migrate RAILS_ENV="production"
    rake generate_session_store

###2.3  主题

    git clone 到 `/usr/local/www/redmine-2.5.2/public/themes`，到后台配置里面选定

###2.4  插件

    git clone 到 `/usr/local/www/redmine-2.5.2/plugins`

###2.5  完成执行

    rake redmine:plugins:migrate RAILS_ENV=production