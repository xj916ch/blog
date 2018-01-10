---
layout: post
category: "kit"
title:  "在Windows打造自己的Linux环境"
tags: [Windows,Linux]
---

使用MSYS2或者Cygwin都可以，都有自己的软件单独安装方式，比如msys2的pacman。

但是这样也麻烦，其实为了简便起见，只需要安装Git for windows就可以了，它除了包含git外，还内置了基本的Linux命令。

### [MSYS2](www.msys2.org/)

### [Cygwin](www.cygwin.com/)

### [Git for Windows](http://gitforwindows.org/)

安装时最好选择最下面的选项，这样的话在cmd中也可以使用git等命令。

![image](https://github.com/xj916ch/xj916ch.github.io/raw/master/_posts/pictures/kit/windows-linux-environment-installation-tips.png)


注意：

因为windows本身也有find等命令，为了使用习惯的linux find，在环境变量PATH中需要把git bin提前，如`PATH=C:\Program Files\Git\cmd;C:\Program Files\Git\mingw64\bin;C:\Program Files\Git\usr\bin;%SystemRoot%\system32;%SystemRoot%;%SystemRoot%\System32\Wbem;%SYSTEMROOT%\System32\WindowsPowerShell\v1.0\;`


