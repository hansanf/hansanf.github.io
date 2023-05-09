---
title: linux_tips
date: 2021-11-01 20:06:11
tags: linux
---
### linux
for linux tips 

##### 格式化环境变量
```
echo $LD_LIBRARY_PATH|awk -F":" '{for(i=1;i<=NF;i++)print $i}'
```

##### 清空共享内存
```
ipcs|awk '{print $1}'|xargs -I {} sudo ipcrm -M {}
```

##### 查看系统启动时间
```ps -p PID -o lstart```

top 的 TIME 是占用 cpu 时间

(加sudo 权限)修改系统时间和时区: https://www.cnblogs.com/ljy2013/p/4615149.html

##### 磁盘/U盘 挂载
查看磁盘列表 
```
sudo fdisk -l
``` 

挂载 mount

卸载 umount
```
sudo mount 磁盘名称 挂载路径
sudo umount 磁盘名称
比如: 
sudo mount /dev/sda1 /home/work/
sudo umount /dev/sda1    
```

不小心拔下在复制数据的移动硬盘出现无法加载的错误
```
MFTMirrdoesnotmatchMFTMirrdoesnotmatchMFT (record 0).
Failed to mount '/dev/sda1': Input/output error
NTFS is either inconsistent, or there is a hardware fault, or it's a
SoftRAID/FakeRAID hardware. In the first case run chkdsk /f on Windows
then reboot into Windows twice. The usage of the /f parameter is very
important! If the device is a SoftRAID/FakeRAID then first activate
it and mount a different device under the /dev/mapper/ directory, (e.g.
/dev/mapper/nvidia_eahaabcc1). Please see the 'dmraid' documentation
for more details.
```
LINUX下```sudo ntfsfix /dev/sda1```

##### 输出重定向
标准输入(键盘输入)、标准输出（输出到屏幕）、标准错误（也是输出到屏幕），它们分别对应的文件描述符是0，1，2

2>&1  意思是把 标准错误输出 重定向到 标准输出.

&>file  意思是把标准输出 和 标准错误输出 都重定向到文件file中

##### 查看core-dump信息
https://www.jianshu.com/p/7317910210a4

```ulimit -c unlimited```

设置core路径

方法1：
```
sudo vim /etc/sysctl.conf
    kernel.core_pattern=/tmp/core-%e-%p-%t  
不加路径就在当前路径下生成core文件，与可执行程序的路径无关

执行：sysctl -p 让配置立刻生效。
```

方法2：
```
sudo vi /proc/sys/kernel/core_pattern
```
加载core文件，直接查看程序core dump时的堆栈信息
gdb  app  core_file

##### 磁盘分区和安装文件系统
https://blog.csdn.net/qq_43527718/article/details/122850052

sudo fdisk /dev/nvme0n1

n  添加分区

p 打印分区表 查看分区成功否

w 保存

mkfs -t ext4 /dev/nvme0n1p1  给第一个分区安装文件系统

sudo blkid /dev/nvme0n1p1  查看nvme0n1p1属性

##### 命令行启动向日葵
```
ps -ef | grep sun
root       836     1  0 10:26 ?        00:01:18 /usr/local/sunlogin/bin/oray_rundaemon -m server
root       856   836  1 10:26 ?        00:08:43 /usr/local/sunlogin/bin/sunloginclient -m service

杀掉两个进程后执行
(开机自启动服务):
sudo systemctl start runsunloginclient.service
sudo systemctl enable runsunloginclient.service
```

##### 查看所支持的 nvidia 驱动
ubuntu-drivers devices

recommend 可以直接用 apt 安装

    nvidia 驱动官网下载:
    https://www.nvidia.cn/Download/index.aspx?lang=cn#


##### 安装cuda
在nvidia官网选择需要的cuda版本下载
https://developer.nvidia.com/cuda-10.2-download-archive?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1604&target_type=runfilelocal

安装方式选择runfile

安装后 cuda路径为/usr/local/cuda-XXXX

##### 安装cudnn
在nvidia官网选择需要的cudnn版本下载
https://developer.nvidia.com/rdp/cudnn-archive

下载解压后，将文件夹内cuda/include/里的所有文件拷贝到/usr/local/cuda-10.2/include/文件夹内，将cuda/lib64/里的所有文件拷贝到/usr/local/cuda-10.2/lib64/文件夹内

##### apt
apt 的目录
```
/etc/apt/source.list 源列表，apt update 所使用的源
/var/lib/apt/lists index 位置，即 apt update所更新的包的标签
/var/cache/apt/archive  apt-get install 下载安装包的路径
```

命令
```
apt-cache show 包名，展示的是/var/lib/apt/lists 目录下所对应的标签的信息
apt list 当前所使用的源所能获取到的软件包(deb)
apt install 包
apt remove 包
```
小 tip：当编译提示缺少什么库的时候的，apt list|grep 库 试下, 一般是 libxxx-dev，可以试下 apt install libxxx-dev

