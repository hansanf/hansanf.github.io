---
title: performace-test
date: 2022-08-30 17:53:43
tags:
    - nsight
    - nsys
    - sysbench
    - perf
---

### perf
#### perf 安装
https://xiaoyanzhuo.github.io/2019/01/18/Perf-Tool.html
```
$sudo apt-get update
$sudo apt-get install linux-tools-common linux-tools-generic linux-tools-`uname -r`
```
#### orin上安装

下载 JP-x.x.x Driver Package Sources: 
https://developer.nvidia.com/embedded/l4t/r35_release_v1.0/sources/public_sources.tbz2

执行：
```
复制压缩包到orin
cd到 kernel/kernel-4.9/tools/perf
make
./perf --version
```

### 用法
基础教程：https://access.redhat.com/documentation/zh-cn/red_hat_enterprise_linux/8/html/monitoring_and_managing_system_status_and_performance/common-perf-commands_getting-started-with-perf

用 perf 监控相关指标，生成报告，然后可以将报告用 https://profiler.firefox.com/ 进行查看

步骤：
```
1、sudo perf record -e cpu-clock -g -p PID
2、perf script -i perf.data &> perf.unfold
```
然后将 .unfold 文件在 profiler.firefox.com 网站上打开


### nvidia-system
orin上

/opt/nvidia/nsight-systems/2022.3.3/target-linux-tegra-armv8/nsys profile -y 60 -d 20 --gpuctxsw=true -o out_file mainboard -d ./dag/test.dag

程序开始运行60s后,记录20s.然后core dump 生成记录报告out_file

