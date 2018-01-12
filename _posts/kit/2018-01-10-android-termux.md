---
layout: post
category: "kit"
title:  "Android上模拟Linux运行环境"
tags: [Android, Termux]
---

参考：

[Termux命令行神器初体验](https://www.sfantree.com/termux_01)

[神器Termux的使用日常](https://www.jianshu.com/p/5c8678cef499)

[Hello，Termux](http://tonybai.com/2017/11/09/hello-termux/)

[如何优雅的在手机上写Python](http://www.hackbase.com/article-216177-1.html)

[ubuntu使用SSH通过Termux登录Android设备](http://blog.csdn.net/jy1690229913/article/details/78348440)

### [Termux](https://termux.com/)

- 基本使用

开始的一些操作只能在Termux原始的终端进行，安装zsh后再操作就较为方便。

VOL↑ + Q  调出软键盘

VOL↑ + W  上

VOL↑ + S  下

VOL↑ + A  左

VOL↑ + D  右

- 更新源

`$cd ~`

`$echo "deb [arch=all,arm] http://mirrors.tuna.tsinghua.edu.cn/termux stable main" > ../usr/etc/apt/sources.list`

`$apt update`

- 软件安装

`$pkg install zsh vim openssh openssl-tool curl zip dnsutils`

`$pkg install git clang python`

- 其他

1.启用外置存储

`termux-setup-storage`

执行这条命令，Android6.0以上会弹框确认是否授权，成功拿到存储权限后会在家目录生成storage目录，并且生成若干目录，软连接都指向外置存储卡的相应目录

2.anroid手机主机互相登录：

Termux终端中使用ssh访问远程服务器与Linux终端中使用ssh别无二致。但要使用ssh访问Android设备就不同了，Termux终端中sshd服务不支持密码验证，也就是说用户不能期望通过ssh user@server然后输入用户密码的方式从别的终端访问Android设备。Termux终端中sshd只支持密钥验证。

android和服务器主机处于同一个网段网络。

- 手机登录主机：

如主机ssh一样使用：

比如： `$scp user@192.168.10.111:/home/user/bd.tar.gz .`

- android访问主机

在Termux中启动ssh server：

`$sshd #开启ssh服务`

就这么简单，一个sshd server就在termux后台启动起来了。Termux上sshd默认的listen端口是8022。

另外termux上的sshd server不支持用户名+密码的方式进行登录，只能用免密登录的方式，即将PC上的~/.ssh/id_rsa.pub写入termux上的~/.ssh/authorized_keys文件中。

`cat id_rsa.pub >> $HOME/.ssh/authorized_keys #将设备公钥添加都授权登录列表中`

在termux输入`$whoami`获取username

在termux输入`$ip addr show wlan0`获取ip

其他设备上可通过`ssh username@ip -p port`的方式登录Android设备。

- android JuiceSSH连接termux

juicessh的终端更好看，键盘更好用。

juicessh生成公钥，复制到termux下的.ssh/authorized_keys中。

termux输入sshd打开服务器。

juicessh就可以连接使用了。


