---
title: linux运维
tags: linux运维
categories: linux运维
abbrlink: 17de06a
date: 2019-07-17 17:12:51
---
系统架构设计构建基础学习
<!--more-->
### 常用术语
 PV(Page View)：访问量，用户每刷新一次就会被计算一次。
 UV(Unique Visitor)：独立访问，访问网站的一台电脑客户端为一个访客。
 QPS(Query Per Second)：每秒查询率。是对一个特定的查询服务器在规定时间内所处理流量多少的衡量标准。
 CDN(Content Delivery Network):内容分发网络。目的：通过在现有的Internet中增加一层新的网络架构，将网站内容发布到最接近用户的网络“边缘”，使用户可以就近取得所需要的内容，提高用户访问网站的响应速度。
### 系统基础优化
  本次练习主要采用VM虚拟机，选择Centos7.0操作系统进行最小化安装，学习运维相关知识。
  ping不通，用百度CSDN方法均不行，放弃了。
### VMware学习：
   在安装完虚拟机后，默认安装了如下图的两块虚拟网卡——VMnet1和VMnet8，其中VMnet1是host网卡，用于host方式连接网络；VMnet8是NAT网卡，用于NAT方式连接网络的，并且它们的IP地址是随机生成的。
__桥接模式(Bridge)__

  此模式下，VM centOS 在网络中作为一台独立主机存在，它可以访问网络中的任何一台设备，地位等同宿主机器(如下图，使用VMnet0虚拟网卡，不支持DHCP。


  由于VM作为一台独立主机存在，因此具备自己的TCP/IP地址和子网掩码等，你可以手动配置(必须与宿主机器处于同一个网段)，它可以实现通过局域网的网关/路由器直接访问互联网。

  使用桥接模式的虚拟系统和宿主机器的关系，就像连接在同一个Hub上的两台电脑。想让它们相互通讯，你就需要为虚拟系统配置IP地址和子网掩码，否则就无法通信。

  如果你想利用VMWare在局域网内新建一个虚拟服务器，为局域网用户提供网络服务，就应该选择桥接模式。
__NAT模式__

  使用VMnet8虚拟网卡，支持DHCP。

  NAT(Network Address Translation, 网络地址转换)，就是让VM centOS 借助NAT功能，通过宿主机器所在的网络来访问Internet。换言之，使用NAT模式可以实现在虚拟系统里访问互联网。

  NAT模式下的虚拟系统的TCP/IP配置信息是由VMnet8(NAT)虚拟网络的DHCP服务器提供的(往往不和宿主机在同一个网段)，无法进行手工修改，因此虚拟系统也就无法和本局域网中的其他真实主机进行通讯。

  采用NAT模式最大的优势是虚拟系统接入互联网非常简单，你不需要进行任何其他的配置，只需要宿主机器能访问互联网即可。如果你想利用VMWare安装一个新的虚拟系统，在虚拟系统中不用进行任何手工配置就能直接访问互联网，建议你采用NAT模式。
__仅主机(host-only)__

  使用VMnet1虚拟网卡，支持DHCP，无法访问互联网。
  在某些特殊的网络调试环境中，要求将真实环境和虚拟环境隔离开，这时你就可采用host-only模式。在host-only模式中，所有的虚拟系统是可以相互通信的，但虚拟系统和真实的网络是被隔离开的。

  在host-only模式下，虚拟系统和宿主机器系统是可以相互通信的，相当于这两台机器通过双绞线互连；虚拟系统的TCP/IP配置信息(如IP地址、网关地址、DNS服务器等)，都是由VMnet1(host-only)虚拟网络的DHCP服务器来动态分配的。

__loopback(本地环回)__

  本地环回接口，在网络设备（一般是路由器）上是一种特殊的接口，它不是物理接口，而是逻辑接口（也称虚拟接口），在终端通常被固定设置为127.0.0.1(当然你可以自己更改)。

  通常，我们检查网络时，首先检查此端口，若此端口不通，则表示我们TCP/IP的协议栈有问题，需要重新安装/更新。
__网关 & DNS__

  网关(GateWay),相当于一个门(或者关口)，是一个网络通向另一个网络的IP地址；通常由距离我们终端最近的路由器充当这个角色。

  DNS(Domain Name System,域名服务器)， 简单点的解释就是：你准备访问www.baidu.com，当你输入该网址后enter，该请求将会发送到DNS，将其转换为IP地址，然后才进行网络通信；若没有DNS，你必须记住你想访问网址的IP地址，这一般人做不到。





### Centos7配置网卡连接内网、外网








### CentOS7 配置163 yum源
1. 下载repo文件 
wget http://mirrors.163.com/.help/CentOS7-Base-163.repo
 
2. 备份并替换系统的repo文件 
cp CentOS7-Base-163.repo /etc/yum.repos.d/ 
cd /etc/yum.repos.d/ 
mv CentOS-Base.repo CentOS-Base.repo.bak 
mv CentOS7-Base-163.repo CentOS-Base.repo
 
3. 执行yum源更新命令 
yum clean all 
yum makecache 
yum update
 
配置完毕。