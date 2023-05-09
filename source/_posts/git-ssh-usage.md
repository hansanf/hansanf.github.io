---
title: git_ssh_usage
date: 2021-11-26 20:35:17
tags: 
    - git
    - ssh
---

### git在线练习平台
[https://learngitbranching.js.org](https://learngitbranching.js.org)

[git常用命令和基本概念](https://mp.weixin.qq.com/s/sp1YUQ2vnQaIGH4tO3j1Vw)

### 代码托管平台push 时需要的 ssh 密钥
#### ssh 密钥生成
1. 生成ssh密钥  ssh-keygen -t rsa   可以直接一路回车
2. 打印密钥内容  cat ~/.ssh/id_rsa.pub
3. 复制密钥到托管平台
4. 查看.ssh/config文件，是否配置了你的ssh，没有配置不会被使用（在多人使用的情况下要注意）

#### ssh 配置
##### 多个用户时，指定用户在代码托管平台所用的密钥

ssh-keygen 生成密钥时指定名称，比如加个后缀 .github来表示该密钥是用于github

```bash 
Host github.com
    User hansanf
    IdentityFile ~/.ssh/id_rsa.github
```

###### ssh 连接远程主机时别名登录和免密登录
```bash
  1 Host fgq
  2     HostName 主机端ip
  3     User 主机端用户名
  4     Port 22
  5     IdentityFile ~/.ssh/id_rsa.remote  # 连接主机所用的密钥，需要将公钥放在主机端
```
将公钥放在主机端 ~/.ssh/ 目录下使用 ```ssh-copy-id -i ~/.ssh/id_rsa.pub username@192.168.11.11```，或scp 传输到 .ssh 目录下, 然后就可以通过 ```ssh fgq``` 登录远程主机了

###### ssh 通过跳板机登录远程主机
```bash
Host fgq
    HostName 192.168.2.100
    User caros
    Port 22
    ProxyCommand ssh middle@relay  %h:%p

Host relay
    HostName 192.168.1.10
    User middle
    Port 22
```
上述配置实现```ssh fgq```时先登录relay 机器，然后再登录到 fgq 主机。


### 配置文件
#### 全局配置文件
全局配置文件有`~/.gitconfig`和`~/.git-credentials`两个

`~/.gitconfig`对应着`git config --global`命令。

```bash
//查
git config --global --list
 
git config --global user.name
 
//增
git config  --global --add user.name fgq
 
//删
git config  --global --unset user.name
 
//改
git config --global user.name fgq
```
比如修改GitHub账号名

```bash
git config  --global  user.name hansanf
```
`~/.gitconfig`文件就会相应的做出修改
![在这里插入图片描述](https://img-blog.csdnimg.cn/6e26501083b945e1a65f2c225387b949.png)

`~/.git-credentials`中文是资格证书，里面保存github的token，使每次登陆都可以免密, 该部分应该是https token,也可以直接使用上面介绍的 ssh 配置。

需要注意的是, 在hexo 的 deploy (_config.yml) 中, 
```yaml
deploy:
    type: git
    repo: git@github.com:hansanf/hansanf.github.io.git    # ssh 方式, 需要设置 ssh
    #repo: https://github.com/hansanf/hansanf.github.io   #https 方式, 需要设置 git-credentials 
    branch: master
```
repo 的地址可以在 github 的 repo 页面，点击 Code, 查看对应的 https 地址和ssh 地址


```bash
https://GitHub用户名:具体的token@github.com
```

#### 局部配置文件
在使用 `git init 文件目录`命令配置的git工作区中，即`文件目录/.git/config`，是局部配置文件，对应着`git config --local`命令。

`.gitignore`也是只对当前工作区起作用，可以把要忽略的文件名填进去，然后Git就会自动忽略这些文件

示例：

```bash
# 忽略所有.开头的隐藏文件:
.*
# 忽略所有.class文件:
*.class

# 不忽略.gitignore和App.class:
!.gitignore
!App.class
```

### git 命令
#### 把修改/删除/新建文件添加到暂存区 
-  修改和删除的  `git add -u`  --update
- 修改和新建的  `git add .` 
- 修改、删除和新建的 `git add -A` -ALL


#### add后的东西撤销
- `git status` 查看暂存区有哪些文件
- `git reset HEAD` 暂存区所有文件都撤销
- `git reset HEAD /XXX/XXX.cpp` 撤销特定的文件
- git checkout -- 

恢复到前一个commit, 并且将当前commit 的提交全部变为 绿色 ```git reset --soft HEAD^```

恢复到前一个commit，并且将当前commit 的提交全部变为 红色 ```git reset --mixed HEAD^```

--soft 和 --mixed 不会修改文件内容，只会修改文件在仓库的状态

将某个文件从当前提交中删除, 有一条删除该文件的修改，但文件内容不会变 ```git rm --cache test.txt```

#### 提交到本地仓库（repository）
`git commit -m "记录版本信息"` 提交暂存区文件到本地仓库
`git log` 查看commit-id和所有的版本信息

#### 提交仓库后，再次修改而不保留上次commit信息
- `git commit --amend` 
在本地仓库内容没有合并入远程分支时，可以给上次commit的内容打补丁。打完补丁后，上次的commit-id和其版本信息都被本次的amend所覆盖
加上 --no-edit 可直接跳过修改comment


#### 推到远程仓库，合入master
- `git push origin HEAD:refs/for/master `
origin是远程仓库的代名词
master远程仓库的分支

#### 避免push到远程合入时有冲突，先拉远程最新分支
- `git stash` 把本地修改的内容保存到堆栈中
- `git pull` 拉取远程最新代码，并merge到本地HEAD  `git pull = git fetch ; git merge `
- `git stash pop` 修改内容从堆栈弹出
然后再 add，commit。这样可以基于当前的最新内容进行更改


#### merge时有冲突，手动修改冲突内容
打开冲突文件
```
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```
======= 是两个版本的冲突内容，上面是当前版本，下面是别的分支修改后的版本，删掉不需要的内容，和所有==、>>·HEAD。

TODO: 怎么用还不太确定 
    自动修改冲突，使用 git checkout --ours 或者 git checkout --theirs

#### 单独克隆一个分支 
git clone --branch 分支名 --depth 分支深度 
单独克隆一个分支后git pull只能拉取当前分支的内容，若需要其他分支，需要先fetch到本地：git fetch origin [remote-branch]:[local-branch]


#### 打补丁
制作补丁：git diff > xxx.patch
检查patch文件：git apply --stat xxx.patch
查看补丁是否能够干净顺利地应用到当前分支中: git apply --check xxx.patch
将补丁合入当前分支 git apply xxx.patch


### git error 解决
#### fatal: refusing to merge unrelated histories
https://www.educative.io/answers/the-fatal-refusing-to-merge-unrelated-histories-git-error

```git pull origin master --allow-unrelated-histories```



参考：
[修改git config配置文件](https://blog.csdn.net/themagickeyjianan/article/details/79683980?spm=1001.2014.3001.5506)

[Git 中的.gitignore文件的作用及配置](https://blog.csdn.net/Lakers2015/article/details/111990909)


