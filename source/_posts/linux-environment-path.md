---
title: 环境变量 
date: 2021-11-25 19:50:12
tags: linux
---

## 环境变量
### 三个文件
#### 1. `/etc/profile`
系统级别，可以所有用户起作用，网上有说是在用户登录时读取的。但在我电脑上使用`source /etc/profile`后只在当前终端起作用，新建终端仍不生效，为了以后在每个终端上都生效可以在`~/.bashrc`中添加`source /etc/profile`。
注意`~/.bashrc`和`/etc/profile`中还包括对终端其他方面的设置，比如显示格式、颜色

#### 2. `/etc/environment`
系统级别，应该是专门用于设置环境变量的，据说优先级高于`/etc/profile`，没验证过。
#### 3. `~/.bashrc`
只对当前用户生效，在个人电脑上直接修改这个文件即可，简单省事。

### 环境变量书写格式
最好在文件末尾添加
```bash
# 其他内容
PATH=$PATH:/usr/local/bin:/usr/bin
```

`PATH`表示环境变量，等号两边不要有空格（shell语法，赋值时等号两边不能有空格）
`$PATH`表示取变量值，即如果上面定义了`PATH`，可以直接引用上面定义好的环境变量，所以这也是为什么要加在文件末尾
`:`为分隔符，每一个环境变量的路径都要用`:`分开

上面这段代码等价于

```bash
#其他内容
PATH=$PATH:/usr/local/bin
PATH=$PATH:/usr/bin
```
修改好后，使用source使其生效

### 3.export命令
export命令使环境变量在当前终端生效，关闭终端后失效。

```bash
export PATH=$PATH:环境变量路径
```
注意，一定要加上`$PATH`,否则会使已有的环境变量全都失效，只剩下新添加的
