title: hexo博客搭建
tags:
  - nodejs
date: 2014-11-25 13:18:02
---

##一、hexo环境安装

安装nodejs环境

执行`npm install hexo -g`

<!-- more -->
##二、hexo博客创建

>hexo init ihww-blog

##三、主题安装

**[主题下载](https://github.com/hexojs/hexo/wiki/Themes)**

在theme目录下

>git clone https://github.com/hexojs/hexo-theme-light.git

修改`_config.yml` 下的theme参数修改为 `hexo-theme-light`

    theme: hexo-theme-light


##四、发布到github

修改`_config.yml`

    deploy:
      type: github
      repo: git@github.com:ihuangweiwei/ihww-blog.git

执行

    hexo deploy

##五、自定义域名

1.  dns服务CNAME到github提供的域名
2.  在source目录下创建CANME文件，填写自己的域名`blog.ihww.cn`

