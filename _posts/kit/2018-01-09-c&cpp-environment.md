---
layout: post
category: "kit"
title:  "C&CPP编译环境"
tags: [c]
---

参考：

[Clang+llvm windows运行环境配置](http://blog.csdn.net/gocad/article/details/42297829)

### [Code::Blocks](http://www.codeblocks.org/)

### VS Code

1. VS Code配置

2. VS Code断点调试

### NPP

NPP配置

### LLVM配置

添加环境变量：

```
@remset run llvm+clang with mingw path env  
@echooff  
set MINGW_HOME=c:\LLVM
set MINGW_VERSION=7.2.0
set "PATH=%MINGW_HOME%\bin;%PATH%"
set "C_INCLUDE_PATH=%MINGW_HOME%\include;%MINGW_HOME%\x86_64-w64-mingw32\include"
set "CPLUS_INCLUDE_PATH=%C_INCLUDE_PATH%;%MINGW_HOME%\lib\gcc\x86_64-w64-mingw32\%MINGW_VERSION%\include;%MINGW_HOME%\lib\gcc\x86_64-w64-mingw32\%MINGW_VERSION%\include\c++;%MINGW_HOME%\lib\gcc\x86_64-w64-mingw32\%MINGW_VERSION%\include\c++\x86_64-w64-mingw32;%MINGW_HOME%\lib\gcc\x86_64-w64-mingw32\%MINGW_VERSION%\include\c++\backward"
set LLVM_HOME=c:\LLVM
set "PATH=%LLVM_HOME%\bin;%PATH%"
%comspec%
```

