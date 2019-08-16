---
title: git学习手册
categories: git命令学习
tags: Git
abbrlink: 9136e18a
date: 2019-07-07 14:27:57
---
记录git常用操作及基本原理
<!--more-->
## git init
**创建一个gittest文件夹,使用git init命令初始化一个空的Git仓库**
![](https://github.com/starstarb/clouding/raw/master/git/9[R6MUG`HK4GWX7C5YNM~3S.png)

## git add&&git commit
**创建一个test.txt文件，通过git add跟踪该文件，然后使用git commit提交更新到仓库**

![](https://github.com/starstarb/clouding/raw/master/git/{PRMIJ{{94A0G(Y06}[@3(E.png)

## 查看状态 && 暂存区

git status命令可以查看仓库状态，列出当前目录所有还没有被git管理追踪的文件和被git管理且被修改但还未提交更新的文件.。

### 文件状态

仓库中的文件可能存在于这三种状态：

- Untracked files → 文件未被跟踪；

- Changes to be committed → 文件已暂存，这是下次提交的内容；

- Changes not staged for commit → 已跟踪文件的内容发生了变化，但还没有放到暂存区。

通过git add的文件会加入暂存区，之后git commit会将暂存区的所有文件提交更新。

![](https://github.com/starstarb/clouding/raw/master/git/t017547ec5ae4321b5c.jpg)

## Git对象和目录

### Git对象

在Git系统中有四中类型的对象，所有的Git操作都是基于这四种类型的对象。

- "blob"，这种对象用来保存文件的内容。

- "tree"，可以理解成一个对象关系树，它管理一些"tree"和“blob”对象。

- "commit"，指向一个"tree"，它用来标记项目某一个特定时间点的状态。它包括以下关于时间点的元数据，如时间戳、最近一次提交的作者、指向上次提交、

- "tag"，给某个提交增添一个标记。


### SHA1哈希值

在Git系统中，每个Git对象都通过哈希值来代表这个对象。哈希值是通过SHA1算法计算出来的，长度为40个字符（40-digit）。

### Commit 对象

commit对象一般包含以下信息：

- 代表commit的哈希值

- 指向tree对象的哈希值

- 作者

- 提交者

- 注释
![](https://github.com/starstarb/clouding/raw/master/git/t01af2028942c47f552.png)

### Tree 对象

Tree对象一般包含以下信息：

- 代表blog的哈希值

- 指向tree对象的哈希值
![](https://github.com/starstarb/clouding/raw/master/git/t014945927507f151f0.png)

### Blob 对象

Blob对象一般包含用来存储文件的内容。

![](https://github.com/starstarb/clouding/raw/master/git/t0121e26267d17bb702.png)

### Tag 对象

标签对象一般包含以下信息：

- 一个对象名(SHA1签名)

- 对象类型

### 对象间的关系

commit、tree、blob三个对象的简单关系如下：
![](https://github.com/starstarb/clouding/raw/master/git/t017c30a93b25c79581.jpg)

### 若仓库的目录结构如下：
```
|-- README

`-- lib

   |-- inc

   |   `-- tricks.rb

   `-- mylib.rb
```
git对象结构的关系图如下：
![](https://github.com/starstarb/clouding/raw/master/git/t016b6fd3aca4e8a774.png)


## .git目录

在我们使用git init初始化git仓库的时候，会生成一个.git的隐藏目录，git会将所有的文件，目录，提交等转化为git对象，压缩存储在这个文件夹当中。

- COMMIT_EDITMSG：保存最新的commit message，Git系统不会用到这个文件，只是给用户一个参考

- config：这个是GIt仓库的配置文件

- description：仓库的描述信息，主要给gitweb等git托管系统使用

- HEAD：这个文件包含了一个档期分支（branch）的引用，通过这个文件Git可以得到下一次commit的parent

- hooks：这个目录存放一些shell脚本，可以设置特定的git命令后触发相应的脚本；在搭建gitweb系统或其他

git托管系统会经常用到hook script

- index：这个文件就是我们前面提到的暂存区（stage），是一个二进制文件

- info：包含仓库的一些信息

- logs：保存所有更新的引用记录

- objects：所有的Git对象都会存放在这个目录中，对象的SHA1哈希值的前两位是文件夹名称，后38位作为对象文件名

- refs：这个目录一般包括三个子文件夹，heads、remotes和tags，heads中的文件标识了项目中的各个分支指向的当前commit

- ORIG_HEAD：HEAD指针的前一个状态

### Git引用

Git中的引用是一个非常重要的概念，对于理解分支（branch）、HEAD指针以及reflog非常有帮助。

Git系统中的分支名、远程分支名、tag等都是指向某个commit的引用。比如master分支，origin/master远程分支，命名为V1.0.0.0的tag等都是引用，它们通过该保存某个commit的SHA1哈希值指向某个commit

### HEAD

HEAD也是一个引用，一般情况下间接指向你当前所在的分支的最新的commit上。HEAD跟Git中一般的引用不同，它并不包含某个commit的SHA1哈希值，而是包含当前所在的分支，所有HEAD直接执行当前所在的分支，然后间接指向当前所在分支的最新提交。

查看当前分支，当前分支为master，并通过git log可以发现这个commit就是master分支上最新的提交。
![](https://github.com/starstarb/clouding/raw/master/git/43@$H0K}7TGN]Y5]ZZFL1BG.png)

所有的内容都是环环相扣的，我们通过HEAD找到一个当前分支，然后通过当前分支的引用找到最新的commit，然后通过commit可以找到整个对象关系模型：  
![](https://github.com/starstarb/clouding/raw/master/git/t012a042a5c574ca67f.jpg)

### 引用和分支

- git branch test，创建名为test分支。

- git checkout test，切换到test分支。

除了master分支，创建了一个test的分支，切换到新分支，查看HEAD，HEAD的指向发生了变化。

再次查看“.git/refs/heads/”目录，可以看到除了master文件之外，又多了一个test文件，查看该文件的内容也是一个哈希值。
![](https://github.com/starstarb/clouding/raw/master/git/branch.png)

- git show-ref --heads，命令就可以查看所有的头
![](https://github.com/starstarb/clouding/raw/master/git/view.png)

### 日志

进入.git/logs文件夹，可以看到这个文件夹也有一个HEAD文件和refs目录，这些就是记录仓库修改的地方。

目录下记录了包括git commit，git checkout，git stash等命令的操作历史。

git stash，保存当前工作进度，会把暂存区和工作区的改动保存起来。

执行完这个命令后，在运行git status命令，就会发现当前是一个干净的工作区，没有任何改动。

当然可以使用git stash pop来恢复之前的进度.

![](https://github.com/starstarb/clouding/raw/master/git/log.png)

### 索引index

index/stage，就是更新的暂存区，看看index文件。

index（索引）是一个存放了已排序的路径的二进制文件，并且每个路径都对应一个SHA1哈希值。在Git系统中，可以通过git ls-files --stage来显示index文件的内容。

创建新文件，添加到暂存区(SHA1哈希值已经改变)
![](https://github.com/starstarb/clouding/raw/master/git/20190816103129.png)

### 对象的存储

所有的Git对象都会存放在.git/objects目录中，对象SHA1哈希值的前两位是文件夹名称，后38位作为对象文件名。

![](https://github.com/starstarb/clouding/raw/master/git/20190816103033.png)

