---
title: GFLAG_usage
date: 2023-02-14 14:42:53
tags:
    - gflag
---

### GFLAG使用记录

在main()中调用gflags::ParseCommandLineFlags(&argc, &argv, true) 解析传入GFLAGS_xxx，在运行bin文件的时候，可以选择用命令传入flags参数，或者将参数放在一个文本文件中

示例：
```
// main.cpp
#include <iostream>
#include "gflags/gflags.h"
DEFINE_string(local_ip, "127.0.0.1", "genenral local ip");

int main(int argc, char** argv){
    gflags::ParseCommandLineFlags(&argc, &argv, true);   // 解析GFLAS_XX 
    std::cout << FLAGS_local_ip << std::endl; 
    return 0;
}
```

g++ main.cpp 生成a.out可执行文件后，
命令行传入FLAGS_local_ip: 
```./a.out --local_ip=127.0.0.2```

文本文件传入：
```
// flags.txt
--local_ip=127.0.0.2
```
./a.out --flagfile=./flags.txt

如果不设置local_ip参数，默认打印 127.0.0.1；设置locol_ip参数后打印 127.0.0.2

**Note**:

define有且只能出现一次，所以当头文件会被多个文件包含时，一定不能定义在头文件中
因此，DEFINE在cpp中，DECLARE在头文件中，若只想被一个.cpp文件使用，则只定义在此.cpp文件中而不在.h文件中声明
