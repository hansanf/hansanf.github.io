---
title: C++和shell脚本的输入参数设置
date: 2022-11-21 15:57:06
tags:
---

## C++ 可执行文件输入参数

### 1. getopt_long()函数
使用手册： https://linux.die.net/man/3/getopt_long

### 2. 设置参数选项
参数选项的形式可以分为两种，一是 -n, 二是 --name

-n 这种需要设置短选项字符串；--name 这种需要设置长选项结构体。

#### 2.1 段选项字符串

书写规则：
- 多个短选项可以连在一起
- 如果某个要解析的选项需要一个参数，则在选项名后面跟一个冒号
- 如果某个要解析的选项的参数可选，则在选项名后面跟两个冒号

一般如这种形式:
    ```const char* short_options = "hx:t:c:"```
该字符串解释为： 

    -h 

    -t t_arg

    -c c_arg

细心的话为发现多了h后面还有一个x:没有被说明，因为h(help)后面不需要接参数

```cpp
const char* short_options = "hx:n:h:";    // 带参数的，要使用的命令，需要在这里声明
const struct option long_options[] = {
        {"help", 0, NULL, 'h'},
        {"decode", 1, NULL, 'd'},
        {"encode", 1, NULL, 'e'},
    {"output", 1, NULL, 'o'},
        {nullptr, 0, nullptr, 0}
};
```