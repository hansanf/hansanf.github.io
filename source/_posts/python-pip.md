---
title: python-pip
date: 2022-08-28 15:26:39
tags:
    - python
---

### 前言
有条件直接使用 conda的虚拟环境

### miniconda
安装miniconda：https://www.jianshu.com/p/992cadf45994
```
conda env list  查看已有的python 环境
conda create --name py36 python=3.6.2 指定 Python 版本创建虚拟环境
conda activate py36   激活 py36 的虚拟环境
```

### python2安装pip
wget https://bootstrap.pypa.io/pip/2.7/get-pip.py
下载成功后执行 python2 get-pip.py

指定python 安装numpy库：
python2 -m pip install numpy

### python3安装 pip
apt install python3-pip

