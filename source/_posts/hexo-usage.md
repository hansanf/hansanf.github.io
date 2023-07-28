---
title: hexo_usage
date: 2022-05-06 10:23:53
tags: 
	- hexo
	- github
---


### 部署hexo环境
1. 首先安装node.js和Git
```
brew install node.js
brew install git
#查看安装的版本
node -v
git --version
```
2. 安装hexo
```
npm install -g hexo-cli
```
3. 创建博客目录，初始化
```
mkdir blog
cd blog
hexo init
```

### 移植到另一台电脑
1. 备份好原来的 blog 目录 => old_blog
2. 新电脑配置好 hexo 环境，并新建好blog 目录 => new_blog
3. 将 old_blog 目录下的 _conifg.yml、source、和themes 复制到 new_blog 目录
Note：可以将blog 目录直接备份在github repo 中，可以很方便的进行移植。
4. 验证本地版本是否可用：```hexo g; hexo s```  
5. 此时还不能远程推送到git repo, 需要安装hexo-deployer-git插件
```
npm install hexo-deployer-git --save
```
6. 如果是ssh 方式登录github 需要上传新电脑公钥到github
7. Work Done

### hexo集成到docker镜像
[docker-hexo](https://gitee.com/LakeVilladom/docker-hexo)

