---
title: protobuf
date: 2022-06-29 11:25:30
tags: 
	- C++
	- protobuf
---

##  什么是protobuf
Protocol Buffers(简称Protobuf) ，是Google出品的序列化框架。
简单来讲，就是支持序列化与反序列化，用于数据的存储、传输。
protobuf将数据接口定义在.proto文件中，然后再利用protoc翻译为所需要的程序语言代码（类似于接口描述语言？）

[官方Guide](https://developers.google.com/protocol-buffers/docs/cpptutorial)
## 怎么用protobuf
### 安装
...省略
### C++使用
#### 数据类型定义
[数据类型对应关系](https://blog.csdn.net/wangchong_fly/article/details/47614699)
#### demo
step-1定义.proto
```cpp
// demo_msg.proto
syntax = "proto2"; //版本选择，proto2 or proto3
package fgq.demo; // C++中的namespace,用.分隔

message PersonalInfo {
	required bytes name = 1;
	optional bytes address = 2;
	repeated bytes friends = 3;
};
//等号后面是field number,从1开始计数
//C++中字段名不区分大小写，即name和Name，在C++接口中都是name
```
step-2 将.proto翻译为.cpp和.h

```cpp
protoc -I= demo_msg.proto的目录 --cpp_out=存放.cpp和.h的目录 demo_msg.proto的路径
```
-I参数和gcc的-I参数类似？

step-3
.cpp中引入.pb.h头文件，就可以使用protobuf所提供的cpp接口了

```cpp
// main.cpp
#include "demo_msg.pb.h"
int main(){
	...
}
```

```cpp
g++ main.cpp -I [--cpp_out所指定的目录]
```
