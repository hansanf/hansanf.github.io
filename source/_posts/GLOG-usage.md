---
title: GLOG_usage
date: 2023-02-14 14:45:23
tags:
    - glog
---
```
头文件包含：
#include "glog/logging.h"   // 添加头文件

初始化：
FLAGS_log_dir = "./";  // 指定地址log文件路径，默认是在/tmp/
google::InitGoogleLogging(argv[0]);   // 设置log文件名称，argv[0].INFO

记录信息：
LOG(WARNING) << "WARNING: this is a test for glog"; 
LOG(INFO) << "INFO: this is a test for glog";
```

还有更高阶用法：https://rpg.ifi.uzh.ch/docs/glog.html


https://my.oschina.net/u/4320185/blog/3755592
