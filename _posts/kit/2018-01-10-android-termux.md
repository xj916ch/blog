---
layout: post
category: "kit"
title:  "Android上模拟Linux运行环境"
tags: [Android, Termux]
---

参考：

[Termux命令行神器初体验](https://www.sfantree.com/termux_01)

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

1.拷贝同一连接网络内其他PC主机数据到手机：

比如： `$scp user@192.168.10.111:/home/user/bd.tar.gz .`



