---
title: gcc-options
date: 2022-07-14 14:11:09
tags:
    - gcc
---

### gcc 的编译选项
gcc官方手册：https://gcc.gnu.org/onlinedocs/

7.5手册：https://gcc.gnu.org/onlinedocs/gcc-7.5.0/gcc/#toc-GCC-Command-Options

链接选项：https://gcc.gnu.org/onlinedocs/gcc-7.5.0/gcc/Link-Options.html#Link-Options

#### -fPIC 和-fPIE
-fPIC 用在动态库的编译，产生位置无关的代码

-fPIE 用在可执行程序的编译

两者在效果上一样，都是把代码中的逻辑地址转为相对地址，但是作用上不一样。
-fPIC一般是动态库编译必须设置的，因为可能在链接时，多个模块之间的重定向可能会出现冲突。而-fPIE在可执行程序的编译中可加可不加，其将绝对地址转为相对地址，在一定程度上提高了安全性，另外相对寻址的方式可能会让程序在启动速度慢一点。

PIE 详解：https://www.redhat.com/en/blog/position-independent-executables-pie
