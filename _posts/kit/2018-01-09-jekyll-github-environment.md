---
layout: post
category: "kit"
title:  "jekyll + Github Pages设置"
tags: [jekyll,Github Pages]
---

参考：

[Github Pages + Jekyll 独立博客一小时快速搭建&上线指南](http://playingfingers.com/2016/03/26/build-a-blog/)

[搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)

[在Github上搭建Jekyll博客和创建主题](http://blog.csdn.net/garfielder007/article/details/50224513)

[怎么搭建个人博客网站——我的建站过程](https://www.cnblogs.com/shenjieblog/p/5060927.html)

### jekyll安装
1. 安装[Ruby](https://rubyinstaller.org/downloads/)，记得添加到环境变量。
2. 安装[RubyGems](https://rubygems.org/pages/download)，下载ZIP文件解压，输入：
```
$cd {unzip-path}
$ruby setup.rb
```
3. 安装jekyll，这里可以更改[Ruby源](https://gems.ruby-china.org/)。
```
$gem install jekyll
$gem install jekyll-paginate
```


### jekyll配置

1. jekyll使用
```
$jekyll new blog
$jekyll server
```

浏览器打开http://localhost:4000/

2. 其他注意配置：

timezone: Asia/Shanghai


### 域名绑定
