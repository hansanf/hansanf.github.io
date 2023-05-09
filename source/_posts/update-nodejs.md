---
title: update_nodejs
date: 2021-11-01 20:49:16
tags: linux
---
## Ubuntu升级nodejs
### 下载nodejs压缩文件
打开nodejs官网，打开`DOWNLOADS`页面，选择一个版本，右键复制链接地址，然后使用命令

```bash
wget https://nodejs.org/dist/v16.13.0/node-v16.13.0-linux-x64.tar.xz 
```
下载到本地
### 解压

```bash
tar -xvf v16.13.0/node-v16.13.0-linux-x64.tar.xz 
```
### 将node和npm设置为全局
将新的node可执行文件**硬链接**到/usr/local/bin/node,如果提示连接已存在，可将/usr/local/bin/node删掉，再重新连接(删除前建议先备份)
```bash
sudo ln 解压后路径/node-v16.13.0-linux-x64/bin/node /usr/local/bin/node
 
sudo ln 解压后路径/node-v16.13.0-linux-x64/bin/npm /usr/local/bin/npm  
```
**/bin/usr和/bin/local/usr的区别：**
&emsp;&emsp; /usr/bin下面的都是系统预装的可执行程序，会随着系统升级而改变。
&emsp;&emsp; /usr/local/bin目录是给用户放置自己的可执行程序的地方，推荐放在这里，不会被系统升级而覆盖同名文件。
&emsp;&emsp; /如果两个目录下有相同的可执行程序，谁优先执行受到PATH环境变量的影响

```bash
fgq@ubuntu:~/hexo_blog$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```
&emsp;&emsp; 这里/usr/local/bin优先于/usr/bin, 一般都是如此

**另外一种设为全局的方法：使用别名alias** 

当前终端生效：
```bash
alias node=解压后路径/node-v16.13.0-linux-x64/bin/node
alias npm=解压后路径/node-v16.13.0-linux-x64/bin/npm
```
永久生效：
&emsp;&emsp;修改主目录下.bashrc文件(~/.bashrc)，添加上述两句
&emsp;&emsp; 然后 `source  ~/.bashrc`
**note:** 等号两边没有空格 

> node和nodejs之间没有区别，node全称就是nodejs。nodejs是一个基于Chrome V8引擎的JavaScript运行环境

参考：
[1、https://blog.csdn.net/qq_37035946/article/details/99451703](https://blog.csdn.net/qq_37035946/article/details/99451703)
[2、https://blog.csdn.net/nzjdsds/article/details/88345400](https://blog.csdn.net/nzjdsds/article/details/88345400)
