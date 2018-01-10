---
layout: post
category: "kit"
title:  "Windows cmd终端神器"
tags: [Conemu, cmder]
---

参考：

[工具02：cmd的替代品ConEmu+Clink](https://yq.aliyun.com/articles/44512)

[告别CMD.windows终端神器conemu设置](http://blog.csdn.net/m1mory/article/details/72591289)

[Cmder--Windows下命令行利器](http://www.cnblogs.com/zqzjs/archive/2016/12/19/6188605.html)

[Win下必备神器之Cmder](https://jeffjade.com/2016/01/13/2016-01-13-windows-software-cmder/)

 
</br>
### [Conemu](http://conemu.github.io/)

配置项：

1. 中文乱码

在Startup->Environment里添加set LANG=zh_CN.UTF-8


</br>
### [clink](https://mridgers.github.io/clink/)
注入到cmd，这样在cmd下也可以使用Tab键。

    clink_x86 autorun -i


</br>
### [cmder](http://cmder.net/)集大成者
相当于Conemu,clink等的一个工具集。Full版还带有msysgit。

配置项：

1.修改命令行提示符：

进入解压后的cmder的目录，进入vendor,打开clink.lua文件:将第41行中{lamb}修改为$
如下所示：
local cmder_prompt = "\x1b[1;32;40m{cwd} {git}{hg} \n\x1b[1;30;40m{lamb} \x1b[0m"
修改后：
local cmder_prompt = "\x1b[1;32;40m{cwd} {git}{hg} \n\x1b[1;30;40m$ \x1b[0m"

2.自定义alias

cmder\config\user-aliases.cmd





