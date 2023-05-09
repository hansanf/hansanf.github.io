---
title: gcc 的标准库和头文件
date: 2022-07-19 22:55:28
tags:
    - gcc
    - so
    - include
---

### libgcc_s.so.1是什么
[The GCC low-level runtime library](https://gcc.gnu.org/onlinedocs/gccint/Libgcc.html)

会在有需要的时候自动启动的C 运行时库，可以替代某些机器完成整数、浮点运算，还有一些其他的功能。

### 运行时库
应用程序和操作系统之间的桥梁，对操作系统硬件的抽象，包括对IO 操作、程序启动和程序退出、栈等的实现。不同的操作系统对应不同的运行时库，但提供的接口基本一致，比如windows 和linux 的运行时库，都提供fread 功能，但是其实现应该是不一样的，fread 就包括在运行时库中。

### 编译参数顺序
LINKFLAGS_AFTER_LIBS(False)决定cxxflags、ldflags（编译参数和链接参数）是放在库前还是库后，对cmake 和bcloud 而言有用，对gcc 而言没有所谓这些参数之分，都是编译参数，链接的库名也都是编译参数，但是编译参数之间的顺序可能会影响能否编译成功

### libc.so 
/usr/lib/aarch64-linux-gnu/libc.so 是个链接脚本，表示在链接时去链哪个库，可以根据需要选择动态库还是静态库

### 符号集版本
func@GLIBC_2.31 

GLIBC_2.31表示符号集版本，向下兼容，即如果需要func@GLIBC_2.29时也没问题

### 强符号和若符号
符号表中的 STRONG 和 WEAK

强符号：全局且初始化的参数和默认的函数，都属于强符号，强符号全局只能唯一，找不到直接报 undefined  reference

弱符号：全局且未初始化的参数，也可以在代码中来指定。弱符号在链接时候找不到可以暂时用0地址代替，链接时不会报 undefined，作为插件接口或用户自定义接口使用更加方便。

### gcc 安装
https://wenku.baidu.com/view/b73c43055a0102020740be1e650e52ea5418ce77.html?_wkts_=1676342805235

wget https://mirrors.ustc.edu.cn/gnu/gcc/gcc-7.5.0

Note: gcc 安装后 c 和 c++ 的标准库还是原来的版本

### 添加系统头文件搜索路径
https://blog.csdn.net/yjk13703623757/article/details/83154578

gcc的环境变量CPLUS_INCLUDE_PATH（C程序使用的是C_INCLUDE_PATH）

### gcc 低版本问题
 gcc4 gcc5 不支持直接使用enum 作为map 的键指，需要自己指定hash 方法。

 