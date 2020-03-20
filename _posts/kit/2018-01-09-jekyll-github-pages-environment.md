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

[迁移博客图片者的福音：使用GitHub做免费不限流量图床](http://jingpin.jikexueyuan.com/article/36279.html)

[Hexo在Github中搭建博客系统(7)万网域名解析到Github Pages](http://blog.csdn.net/chwshuang/article/details/52350589)

[GitHub Pages 绑定域名](http://blog.csdn.net/u013282507/article/details/54944395)

### jekyll安装
1. 安装[Ruby](https://rubyinstaller.org/downloads/)，记得添加到环境变量。
2. 安装[RubyGems](https://rubygems.org/pages/download)，下载ZIP文件解压，输入：
```shell
$cd rubygems
$ruby setup.rb
```
3. 安装jekyll：
```shell
#更改Ruby源
$gem sources -l
$gem sources -r https://rubygems.org/
$gem sources -a https://gems.ruby-china.com/
```
```shell
#安装jekyll及其他软件
$gem install jekyll
$gem install jekyll-paginate
$gem install tzinfo
$gem install wdm
$gem install zoneinfo
$gem install tzinfo-data
```

### jekyll配置使用

- **jekyll使用**

```shell
$jekyll new blog
$jekyll server --trace
```

浏览器打开http://localhost:4000/出现如下图所示，说明安装成功：
![](https://raw.githubusercontent.com/xj916ch/xj916ch.github.io/master/res/jekyll-github-pages-environment-jekyll-default.png)

提示"Permission denied -bind(2) for 127.0.0.1:4000 (Errno:EACCES)“时，说的是4000端口被占用。

找到端口占用的PID，删掉即可。

```shell
>netstat -ano | findstr 4000
```

jekyll主题可以从http://jekyllthemes.org/网站下载。

- **jekyll配置**

1.时区设置

timezone: Asia/Shanghai

2.插入图片

可以使用Github当作图床。

可以把图片放到blog下的_post目录。这里注意，图片外链地址tree需要替换为raw。

### Github Pages

新建一个github repository，命名为username.githug.io，不能放到其他repo下。

repo setting下找到Github Pages，只能是master branch。

其他一切如git操作。

push前可以使用`jekeyll server`在本地预览验证。

### 域名绑定

ping xxx.github.io进行查询，得到一个IP地址：

比如从ping指令得到一个IP地址151.101.25.147，将这个IP地址记录下来。

在万网注册一个域名，进入【域名控制台】->【域名解析】->【解析设置】。添加解析设置：

添加一个记录类型为A，主机记录为www，解析线路默认，记录值为151.101.25.147的记录。

添加一个记录类型为A，主机记录为@，解析线路默认，记录值为151.101.25.147的记录。

在xxx.github.io repository中添加一个文件CNAME，加入注册的域名如xxx.com。

设置完成，等待几分钟，在浏览器中输入域名，就可以看到Github上的网站了。

