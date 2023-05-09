---
title: compile_runtime_error
date: 2023-02-14 15:04:35
tags:
    - 编译报错
    - 运行报错
    - cpplint报错
---

### 编译时报错
#### 找不到依赖库的头文件
找不到依赖库的头文件，找不到依赖库中头文件所包含的头文件
bcloud 引用依赖库时，头文件需要 HEADER 标签发布到output 中。

发布到output 的头文件，如果包含了库中的其他头文件，也需要一起发布到output 中

为什么之前没有注意这个问题呢？因为一般头文件不会引用自己所不需要的头文件，只有其需要被引用时，才进行 include。而如果将库中的头文件引用操作放在 .cc 文件，其所include 的头文件的内容直接就编译到库中，在其他库所引用该库时，也就不会存头文件找不到另外一个头文件的问题了。

#### undefined reference to `__atan2_finite'
```
[ERROR] fail to compile baidu/adu-lab/framework/output/bin/radar_fusion_app ip:10.61.192.37
 err:bc_out/baidu/adu-lab/framework/app/radar_fusion/baidu_adu-lab_framework_radar_fusion_app_app_radar_fusion.cpp.o: In function `v2x::sensor::radar::radar_fusion::radar_msg_proc(int, std::shared_ptr<os::v2x::device::RadarObstacles const> const&)':
/home/bcloud/bcloud_data/EE/BCLOUD_PROTOBUF/CompileServer/Task/bb84d71b78676b10f34e635275115abf/baidu/adu-lab/framework/app/radar_fusion/radar_fusion.cpp:118: undefined reference to `__atan2_finite'
```

https://github.com/google/filament/issues/2875 
https://stackoverflow.com/questions/62334452/fast-math-cause-undefined-reference-to-pow-finite
https://github.com/google/filament/issues/2146

1、去掉 fast-math                                           
2、或加上 -fno-builtin +#include<tgmath.h>

### 运行时报错
#### undefined symbol
1、运行时报 undefined symbol，可能是编译该so 时没有实际链接依赖库，只是使用了头文件，比如编libmsf_Ucommon_Umath.so时没有链接libopencv_core.so，但还是能编译通过，因为其在系统目录下找到了头文件，有些情况仅使用头文件就编译通过了。。

TODO：具体什么情况只需要头文件

2、
E1219 23:24:25.009349  7549 class_loader_utility.cc:218] [mainboard]LibraryLoadException: /home/caros/work/airos_fusion/baidu/adu-lab/airos/output/3rd/libmodules_Sperception-fusion_Salgorithm_Sfusion_Utracker_Stracker_Sprocess_Slibmsf_Utracker_Uprocess.so: undefined symbol: _ZN9algorithm2ft5track14StateContainer9frequent_E

也是运行期间报 undefined symbol 问题，但 _ZN9algorithm2ft5track14StateContainer9frequent_E 这个符号在libmsf_Utracker_Uprocess.so 这个动态库里的，是作为其中一个类的    静态成员变量， 其使用形式为这种：
StateContainer() : vx_mean_filter_(frequent_), vy_mean_filter_(frequent_){};
其使用静态成员变量在初始化列表中对成员变量进行初始化，因此报错，这种形式应该在一些编译参数的设置下是允许的。

3、undefined symbol 也可能是因为编译链接的库和运行时链接的库版本不一致

### undefined reference google::isGoogleLoggingInited()

从baidu/adu-3rd/glog 引用了glog库
但是本地也有glog库，

笨方法：
删掉本地库后就ok了

卸载glog的方法：
```
//安装
sudo apt-get install libgoogle-glog-dev
//卸载
sudo apt-get remove libgoogle-glog-dev
```
聪明方法：

使用 ldd命令 查看当前所链接的库是哪一个，是否是想要链接的库，使用export命令把想要链接的库放在前面 





### cpplint 报错
#### static extern typedef等要放在声明前
```
template <typename T>
struct select_npy_type {
  const static NPY_TYPES type = NPY_NOTYPE;
};  // Default
```
3行 : Storage-class specifier (static, extern, typedef, etc) should be at the beginning of the declaration

static 放在 const 前面即可