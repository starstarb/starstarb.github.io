---
title: hexo 博客备份
tags: github
abbrlink: b5d2c57b
date: 2019-08-08 15:10:35
---
hexo博客备份指南
![](https://github.com/starstarb/clouding/raw/master/picture/4.jpg)
<!--more-->
## 前言

由于之前忘记备份Hexo博客的markdown文件，在重做系统时候还忘记备份博客了，导致现在不得不重新从网页上扒下来之前的文章重新写一遍，十分耗费精力。因此在网上找了下如何备份Hexo博客，在此记录下。

目前假设Git和Github环境已经配置好了，如果没有配置好详见「Ubuntu16.04下Github配置」。
Git相关操作请参考廖雪峰的Git教程。
### 备份博客

目前假设本地Hexo博客已经初始化。
###创建新分支

在Github.io上建立博客时已经开了一个新仓库了，如果再开另一个仓库存放源代码有点浪费，因此采用建立新分支的方法备份博客。

虽然理论上什么时候创建新分支来备份都可以，但是还是建议在建立博客的时候就创建备份分支。（然而我中途才想起来-.-）

不过在建立新分支前请确保仓库内已有master分支（Hexo本地建站后第一次上传时会自动生成），否则后期再添加master分支比较麻烦（请自行搜索git命令）。

本地Git建立新分支命令如下：

```	

$ git checkout -b BRANCHNAME
```
BRANCHNAME是自定义的新分支的名字，建议起为hexo。
建立.gitignore

建立.gitignore文件将不需要备份的文件屏蔽。个人的.gitignore文件如下：
```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```
在Github上备份

通过如下命令将本地文件备份到Github上。

假设目前在hexo博客的根目录下。

```
$ git add .
$ git commit -m "Backup"
$ git push origin hexo
```
这样就备份完博客了且在Github上能看到两个分支(master和hexo)。
设置默认分支

在Github上你的github.io仓库中设置默认分支为hexo。这样有助于之后恢复博客。master分支时默认的博客静态页面分支，在之后恢复博客的时候并不需要。
个人备份习惯

个人而言习惯于先备份文件再生成博客。即先执行**git add .,git commit -m "Backup",git push origin hexo**将博客备份完成，然后执行**hexo g -d**发布博客。
恢复博客

目前假设本地Hexo博客基础环境已经搭好。
克隆项目到本地

输入下列命令克隆博客必须文件(hexo分支)：

```
$ git clone https://github.com/yourgithubname/yourgithubname.github.io
```
### 恢复博客

在克隆的那个文件夹下输入如下命令恢复博客：

```
$ npm install hexo-cli
$ npm install
$ npm install hexo-deployer-git
```
在此不需要执行hexo init这条指令，因为不是从零搭建起新博客。

完成喵～