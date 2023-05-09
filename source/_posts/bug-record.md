---
title: bug_record
date: 2023-02-13 14:42:35
tags:
---

### 编译问题


### linux 环境问题
问题描述：

程序有许多printf 时，使用管道符或重定向到文件均比直接打印到标准输出的性能要好


原因: https://www.cnblogs.com/lhfcws/p/3197735.html

Console 会给多个进程共享，因此对console操作时会存在进程同步和缓存问题。

ssh远程连接没有该问题，只有在本地运行才有

