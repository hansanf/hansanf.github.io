---
title: orin_env
date: 2023-02-13 17:11:15
tags:
    - orin
---

### jetpack sdk 安装
#### 确认nvidia apt资源列表

cat /etc/apt/sources.list.d/nvidia-l4t-apt-source.list  

确认是否为 

deb https://repo.download.nvidia.com/jetson/common r35.1 main 

deb https://repo.download.nvidia.com/jetson/t234 r35.1 main

#### 校正时间和时区
修改系统时间和时区 

系统时间不准确，可能无法连网、获取nvidia 安装包索引

#### 安装jetpack5.0.2
```
sudo apt update
sudo apt dist-upgrade
sudo reboot
sudo apt-cache show nvidia-jetpack 查看jetpack sdk 版本，确认为5.0.2
sudo apt install nvidia-jetpack
````

#### 查看cuda、cudnn、tensorrt 版本
```
cat /usr/loca/cuda/version.json  
    cuda 11.4.14
cat /usr/include/aarch64-linux-gnu/cudnn_version_v8.h
    cudnn 8.4.1
cat /usr/include/aarch64-linux-gnu/NvInferVersion.h
    tensorrt 8.4.1.5
```

#### 安装jtop
```
sudo apt install python3-pip
sudo -H pip3 install -U jetson-stats
sudo jtop 验证是否可用
```

#### 开启最大性能
```
sudo nvpmodel -m 0 && sudo jetson_clocks
sudo jetson_clocks --fan
jtop 查看 NV Power[0] 是否为MAXN
重启后失效
```

#### wget 报错
```
--2018-10-01 12:11:19--  https://url
Connecting to #:443... connected.
OpenSSL: error:14082174:SSL routines:ssl3_check_cert_and_algorithm:dh key too small
Unable to establish SSL connection.
```

Here's a simple workaround for wget: use wget --cipher 'DEFAULT:!DH' in place of wget.

https://stackoverflow.com/questions/52588948/problem-with-wget-command-ssl3-check-cert

#### 终端重复打印 Message xxx
终端报Message from syslogd kernel:unregister_netdevice: waiting for eth0 to become free

https://blog.51cto.com/xiao987334176/1910715
没有找到链接里说的     #*.emerg      

 16 # provides UDP syslog reception                                          
 17 #module(load="imudp")      
 18 #input(type="imudp" port="514")            
 19                         
 20 # provides TCP syslog reception                                                                
 21 #module(load="imtcp")                                                                                       
 22 #input(type="imtcp" port="514")                                                             
 23                                                 
 24 # provides kernel logging support and enable non-kernel klog messages                                          
 25 #module(load="imklog" permitnonkernelfacility="on")   

把25行注释掉  重启rsyslog服务
/etc/init.d/rsyslog restart