---
title: hexo搭建博客记录
tags: hexo
categories: hexo
abbrlink: 5910dd1d
date: 2019-08-10 09:27:16
---
Hexo+yilia搭建属于自己的博客，新手完全教程
![](https://github.com/starstarb/clouding/raw/master/picture/6.jpg)
<!--more-->
##使用 Hexo 来搭建自己的个人静态博客:
1. Markdown 写博客
2. Hexo 生成 HTML
3. Hexo 发布 HTML 到 Github Pages

>留言功能选择gittalk,不需要备案。

### Install Node 
Hexo 需要 Node
* Mac 安装 Node，可以使用 Homebrew 安装: brew install node
* Windows 安装 Node，进入 https://nodejs.org/en/ 下载安装
* ubuntu 安装 Node,终端运行：
```
sudo apt-get update
sudo apt-get install nodejs
sudo apt-get install npm
```
注意：使用这种方式安装的npm是ubuntu软件仓库中的标准本，如果需要最新版，尽可能去官网下载源码包，[官网地址](http://nodejs.org)

  下载回来后，需要解压缩
  1. ctrl+alt+T打开ubuntu终端
  2. 使用cd 命令进入下载目录，输入`tar zxvf  node-v***.tar.gz`  \* 是指node安装包的完整名称
  3. 解压完成后， cd 进入 node-v\*目录，目录下有configure文件。
  4. 输入`./configure`
  5. 输入 `make`
  6. 输入`sudo make install`
  7. 检测是否安装成功:
  输入：
  ```
  node -v
  npm -v
  ```

####   使用 Node 的淘宝镜像
由于网络的问题，访问 Node 的默认仓库有可能会很慢，很多东西都下载不下来，可以使用淘宝的 Node 的镜像，命令行里执行
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

### Install Github Client
Hexo 和 Github 一起使用就可以搭建一个免费的博客网站
如果没有安装 Git，需要安装一下
到 https://desktop.github.com 下载 Github 客户端

使用github也有很多坑，慢慢摸索，记得搜索windows10下的github配置教程，里面很多教程会指引你配置github，这里给出一个基本教程：

### 首先了解https 和 SSH 的区别：

1、前者可以随意克隆github上的项目，而不管是谁的；而后者则是你必须是你要克隆的项目的拥有者或管理员，且需要先添加 SSH key ，否则无法克隆。

2、https url 在push的时候是需要验证用户名和密码的；而 SSH 在push的时候，是不需要输入用户名的，如果配置SSH key的时候设置了密码，则需要输入密码的，否则直接是不需要输入密码的。

 


####  在 github 上添加 SSH key 的步骤：

* 1、首先需要检查你电脑是否已经有 SSH key  
Windows10一般在这个目录下`C:\Users\PC_2017`,找到.ssh隐藏文件，打开看是否有id_rsa.pub等几个文件。
运行 git Bash 客户端，输入如下代码：
```
$ cd ~/.ssh
$ ls
```
这两个命令就是检查是否已经存在 id_rsa.pub 或 id_dsa.pub 文件，如果文件已经存在，那么你可以跳过步骤2，直接进入步骤3。

* 2、创建一个 SSH key 
```
$ ssh-keygen -t rsa -C "your_email@example.com" 邮箱必须是你的github邮箱
```
代码参数含义：
```
-t 指定密钥类型，默认是 rsa ，可以省略。
-C 设置注释文字，比如邮箱。
-f 指定密钥文件存储文件名。
```
以上代码省略了 -f 参数，因此，运行上面那条命令后会让你输入一个文件名，用于保存刚才生成的 SSH key 代码，如：
```
Generating public/private rsa key pair.
# Enter file in which to save the key (/c/Users/you/.ssh/id_rsa): [Press enter]
```
当然，你也可以不输入文件名，使用默认文件名（推荐），那么就会生成 id_rsa 和 id_rsa.pub 两个秘钥文件。

接着又会提示你输入两次密码（该密码是你push文件的时候要输入的密码，而不是github管理者的密码），

当然，你也可以不输入密码，直接按回车。那么push的时候就不需要输入密码，直接提交到github上了，如：
```
Enter passphrase (empty for no passphrase): 
# Enter same passphrase again:
```
接下来，就会显示如下代码提示，如：
```
Your identification has been saved in /c/Users/you/.ssh/id_rsa.
# Your public key has been saved in /c/Users/you/.ssh/id_rsa.pub.
# The key fingerprint is:
# 01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@example.com
```
当你看到上面这段代码的收，那就说明，你的 SSH key 已经创建成功，你只需要添加到github的SSH key上就可以了。

 

* 3、添加你的 SSH key 到 github上面去

     a、首先你需要拷贝 id_rsa.pub 文件的内容，你可以用编辑器打开文件复制，也可以用git命令复制该文件的内容，如：
```
$ clip < ~/.ssh/id_rsa.pub
```
     b、登录你的github账号，从又上角的设置（ Account Settings ）进入，然后点击菜单栏的 SSH key 进入页面添加 SSH key。

     c、点击 Add SSH key 按钮添加一个 SSH key 。把你复制的 SSH key 代码粘贴到 key 所对应的输入框中，记得 SSH key 代码的前后不要留有空格或者回车。当然，上面的 Title 所对应的输入框你也可以输入一个该 SSH key 显示在 github 上的一个别名。默认的会使用你的邮件名称。

 

* 4、测试一下该SSH key

在git Bash 中输入以下代码
```
$ ssh -T git@github.com
```
当你输入以上代码时，会有一段警告代码，如：
```
The authenticity of host 'github.com (207.97.227.239)' can't be established.
# RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
# Are you sure you want to continue connecting (yes/no)?
```
这是正常的，你输入 yes 回车既可。如果你创建 SSH key 的时候设置了密码，接下来就会提示你输入密码，如：
```
Enter passphrase for key '/c/Users/Administrator/.ssh/id_rsa':
```
当然如果你密码输错了，会再要求你输入，知道对了为止。

注意：输入密码时如果输错一个字就会不正确，使用删除键是无法更正的。

密码正确后你会看到下面这段话，如：
```
Hi username! You've successfully authenticated, but GitHub does not
# provide shell access.
```
如果用户名是正确的,你已经成功设置SSH密钥。如果你看到 “access denied” ，者表示拒绝访问，那么你就需要使用 https 去访问，而不是 SSH 。

另外，如果出现连接失败，一般是端口22被占用，修改443端口即可，参考我的博客中记录的第三次日记。

这里提一点，参考完我的博客还是连接不上的话，记得关闭gitbash端口，等待一会重新打开，重新测试
```
$ ssh -T git@github.com
```
很多次应该是本地环境连接的问题，关闭gitbash重新打开即可正常访问。

#### Install Hexo and initialize Pages
```
$ npm install hexo-cli -g
$ hexo init Blog
$ cd Blog
$ npm install
$ hexo server
```
#### Create a new post

	$ hexo new "My New Post"
#### Run server
写好文章后，先在本地看一下效果，如果满意再发布。进入博客目录，运行 hexo server 后浏览器里访问 http://127.0.0.1:4000 就可以了。

#### Generate static files
```
$ hexo generate
```
### Deploy to git

* 如你在 Github 的名字为 star，在 Github 创建一个名字为 star.github.io 的仓库 (xtuer 修改为你的 Github 名字)
我们博客的网站地址自动为 http://star.github.io
* 安装 hexo 的 git 插件
```
$ npm install hexo-deployer-git --save
```
* 在 Blog/\_ onfig.yml 中配置 git
```
deploy:
    type: git
    repo: git@github.com:star/star.github.io.git
```
* 发布时需要执行下面三条命令
```
$ hexo clean
$ hexo generate
$ hexo deploy
```

> 注意，有时候发布时会提示你没有权限访问 Github 的仓库，那是因为 ssh 访问需要的验证文件无效了，需要更新一下，最简单的就是用 Github 的客户端先访问一下，然后再发布就可以了
使用上面的命令发布好博客后，访问 http://star.github.io 可以看到我们创建的博客能从网络上访问了
[更多信息参考官方文档](https://hexo.io/docs/deployment.html)
### Use tag
Tag 是用来对文章进行分类的:
One tag: tags: Programming
Multi tags: tags: [Programming, Java, Spring]

### Use image
配置 Blog/\_config.yml
```
permalink: :title/
post_asset_folder: true
```
在 Blog/source 下创建图片的目录，如 img，md 中引用图片
```
![](../img/post-asset.png)
```
### 主页显示摘要
在 md 中，摘要内容的后面跟上 <!--more-->，

否则主页会显示文章的全部内容

**用别名简化命令**

```
alias hd='hexo clean; hexo generate; hexo deploy'
alias hs='hexo server -g'
```
**本地预览用 hs**
**发布时使用 hd**

### 域名选择
例如：登录腾讯云，购买域名后，登录到控制台，域名解析列表，进入记录管理，点击添加记录。

1. 主机记录选择两个，www和@
2. 记录类型，CNAME
3. 默认线路
4. 记录值填个人的github域名。
5. 其他操作默认
如图所示:
![](https://github.com/starstarb/clouding/raw/master/data/yuming.png)
 如果没有云主机或服务器，是域名不提供备案的，如果想要备案，可以选择购买一个小空间备案，对小空间进行域名跳转操作，指向github服务器即可。
或者找个认识的朋友(有主机空间的)，帮你备案后，做个转发即可。

### 主题采用yilia
访问官方文档即可：[点击](https://github.com/litten/hexo-theme-yilia)

### 使用github图床管理博客图片
1. 登录github账号。
2. 创建仓库例如: picture
![](https://github.com/starstarb/clouding/raw/master/data/cangku.png)
3. github搜索Picgo，参看官方文档
4. 在github,打开settings,找到Personal access tokens ,点击进去，勾选![](https://github.com/starstarb/clouding/raw/master/data/repo.png)
5. 点击Generate token.
6. 这里会产生一段加密字符串，只会产生一次，记录下来。
7. 在Picgo中选择github图床。
8. 仓库名是yourgithubname/picture
9. 分支名默认master
10. token写刚才产生的那段加密字符串。
11. 存储路径可以自定义为python/

> 写一个字符串加一个斜杠，会自动在github中创建相应路径

12. 自定义域名为
```
https://github.com/yourgithubName/picture/raw/master
```
13. 其他操作直接看Picgo官方文档即可，完成。

> 另外，如果PicGO用不了复制链接，直接进入图片所在路径，将路径中blog更改为raw即可。
