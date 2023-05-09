---
title: cpp_debug_tools
date: 2022-07-05 11:20:37
tags:
    - CPP
    - Debug
---

## C++ Debug命令和工具

### readelf
查看可执行文件或库的符号表
```
readelf -s ./a.out
```
查看可执行文件或库的动态库表
```
readelf -d ./a.out
```

### objdump
查看汇编情况
```
objdump -d ./a.out
```

### ldd
查看动态库的链接情况
```
ldd ./a.out
```

### LD_PRELOAD
在执行该文件前预加载库，要加具体的库名，LD_PRELOAD的值只在当前语句生效
```
env LD_PRELOAD=/home/fenggq/libutil.so ./a.out
```

### 查看系统默认链接的库路径
```
cat /etc/ld.so.conf
    # output: include /etc/ld.so.conf.d/*.conf
cat /etc/ld.so.conf.d/*.conf
    # output: 所有的默认库路径
```

### gdb
使用gdb调试带输入参数的程序
```
gdb --args ./a.out args
```

### file查看文件属性
```bash
$ file msf
msf: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, stripped
```
该文件被stripped ，去除掉了符号表信息

### strip 命令
去掉符号表和调试等信息
[https://blog.51cto.com/u_15614325/5272498](https://blog.51cto.com/u_15614325/5272498)

符号表和strip https://xuanxuanblingbling.github.io/ctf/tools/2019/09/06/symbol/

tmp_LD_PRELOAD=${LD_PRELOAD}
unset LD_PRELOAD
nohup cyber_launch start fusion_tracker.launch &
export LD_PRELOAD=${tmp_LD_PRELOAD}

### core文件去哪了
[core文件去哪了](https://www.jianshu.com/p/7317910210a4)