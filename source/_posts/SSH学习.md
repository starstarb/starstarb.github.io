---
title: SSH学习
copyright: true
tags: ssh
categories: ssh
abbrlink: ea9fff8c
date: 2019-10-06 09:50:41
---
SSH协议
<!--more-->
#### SSH服务
SSH（22端口）是Secure Shell Protocol的简写，由IETF网络工作小组（Network Working Group）制定；在进行数据传输之前，SSH先对联机数据包通过加密技术进行加密处理，加密后在进行数据传输。确保了传递的数据安全。

SSH是专为远程登录会话和其他网络服务提供的安全性协议。利用SSH协议可以有效的防止远程管理过程中的信息泄露问题，在当前的生产环境运维工作中，绝大多数企业普通采用SSH协议服务来代替传统的不安全的远程联机服务软件，如telnet（23端口，非加密的）等。

telnet（23端口）实现远程控制管理，但是不对数据进行加密

在默认状态下，SSH服务主要提供两个服务功能：

（1）一个是提供类似telnet远程联机服务器的服务，即上面提到的SSH服务；

（2）另一个是类似FTP服务的sftp-server，借助SSH协议来传输数据的，提供更安全的SFTP服务（vsftp.proftp）

 **特别提醒：**

 SSH客户端（ssh命令）还包含一个很有用的远程安全拷贝命令scp，也是通过ssh协议工作的。

 SSH(远程连接工具)连接原理：ssh服务是一个守护进程(demon)，系统后台监听客户端的连接，ssh服务端的进程名为sshd,负责实时监听客户端的请求(IP 22端口)，包括公共秘钥等交换等信息。

ssh服务端由2部分组成： openssh(提供ssh服务)    openssl(提供加密的程序)

 ssh的客户端可以用 XSHELL，Securecrt, Mobaxterm等工具进行连接
#### SSH的工作机制
服务器启动的时候自己产生一个密钥(768bit公钥)，本地的ssh客户端发送连接请求到ssh服务器，服务器检查连接点客户端发送的数据和IP地址，确认合法后发送密钥(768bits)给客户端，此时客户端将本地私钥(256bit)和服务器的公钥(768bit)结合成密钥对key(1024bit),发回给服务器端，建立连接通过key-pair数据传输。    
#### SSH的加密技术
加密技术：传输过程，数据加密。            
1.SSH1没有对客户端的秘钥进行校验，很容易被植入恶意代码
2.SSH2增加了一个确认联机正确性的Diffe_Hellman机制，每次数据的传输，Server都会检查数据来源的正确性，避免黑客入侵。                   
  SSH2支持RSA和DSA密钥   
        DSA:digital signature Algorithm  数字签名
        RSA:既可以数字签名又可以加密  

#### SSH知识小结
   1.SSH是安全的加密协议，用于远程连接Linux服务器               
   2.SSH的默认端口是22，安全协议版本是SSH2               
   3.SSH服务器端主要包含2个服务功能SSH连接和SFTP服务器               
   4.SSH客户端包含ssh连接命令和远程拷贝scp命令等 

#### SSH防止登录入侵
    1、用密钥登录，不用密码登陆

    2、牤牛阵法：解决SSH安全问题

       a.防火墙封闭SSH,指定源IP限制(局域网、信任公网)

       b.开启SSH只监听本地内网IP（ListenAddress 172.16.1.61）。

    3、尽量不给服务器外网IP

    4、最小化（软件安装-授权）

    5、给系统的重要文件或命令做一个指纹

    6、给他锁上 chattr +i   +a

#### SSH的功能
   1. 登录                   
       ssh -p22 omd@192.168.25.137               
   2. 直接执行命令  -->最好全路径                   
       ssh root@192.168.25.137 ls -ltr /backup/data                       
           ==>ssh root@192.168.25.137 /bin/ls -ltr /backup/data               
   3. 查看已知主机                    
        cat /root/.ssh/known_hosts
   4. ssh远程执行sudo命令
       ssh -t omd@192.168.25.137 sudo rsync hosts /etc/
 
   5. scp               
             1. 功能   -->远程文件的安全(加密)拷贝                   
                 scp -P22 -r -p /home/omd/h.txt omd@192.168.25.137:/home/omd/               
             2. scp知识小结                   
                 scp是加密远程拷贝，cp为本地拷贝                   
                 可以推送过去，也可以拉过来                   
                 每次都是全量拷贝(效率不高，适合第一次)，增量拷贝用rsync
 
   6. ssh自带的sftp功能               
             1. Window和Linux的传输工具                   
                  wincp   filezip                   
               sftp  -->基于ssh的安全加密传输                   
               samba   
             2. sftp客户端连接                   
                sftp -oPort=22 root@192.168.25.137                   
                put /etc/hosts /tmp                   
                get /etc/hosts /home/omd   
            3. sftp小结：                   
                1.linux下使用命令： sftp -oPort=22 root@x.x.x.x                   
                2.put加客户端本地路径上传                  
                3.get下载服务器端内容到本地                   
                4.远程连接默认连接用户的家目录
##### 后台SSH服务
###### 查询openssl软件
    rpm -qa openssh openssl
###### 查询sshd进程
    ps -ef | grep ssh
        --> /usr/sbin/sshd
###### 查看ssh端口
    netstat -lntup | grep ssh  
    ss | grep ssh                (效果同上，同下，好用)
    netstat -a | grep ssh(记住这个)
    netstat -lnt | grep 22    ==>  查看22端口有没有开/ssh服务有没有开启
    技巧： netstat -lnt | grep ssh | wc -l -->只要大于2个就是ssh服务就是好的
###### 查看ssh的秘钥目录
    ll /root/.ssh/known_hosts  # 当前用户家目录的.ssh目录下
###### ssh的配置文件
    cat /etc/ssh/sshd_config   
###### ssh服务的关闭
    service sshd stop
###### ssh服务的开启：
    service sshd start
###### ssh服务的重启
    service sshd reload    [停止进程后重启] ==> 推荐
    service sshd restart   [干掉进程后重启] ==> 不推荐
###### ssh远程登录
    ssh 192.168.1.100      # 默认利用当前宿主用户的用户名登录
    ssh omd@192.168.1.100  # 利用远程机的用户登录
    ssh omd@192.168.1.100  -o stricthostkeychecking=no # 首次登陆免输yes登录
    ssh omd@192.168.1.100 "ls /home/omd"  # 当前服务器A远程登录服务器B后执行某个命令
    ssh omd@192.168.1.100 -t "sh /home/omd/ftl.sh"  # 当前服务器A远程登录服务器B后执行某个脚本

###### 修改ssh服务的启动文件sshd的几个点
* 1-1  修改 /etc/ssh/sshd_config<\br>   
```   
    GSSAPIAuthentication yes    解决一台服务器管理多个ssh服务
    UseDNS no  加快响应速度因为在内网环境下
    PermitRootLogin no  不运行root用户直接登录
    Port 11544 更改访问端口号
    ListenAddress  192.168.25.130  只监听内网的IP
    Match User anoncvs     当前环境允许登录的用户
    PermitRootLogin no      是否允许root用户登录，一般不允许开
```

* 1-2  重启服务
```
    service sshd restart       写入命令进内存
    service sshd reload(优先)  reload是一个平滑的访问，不影响用户使用
```
* 1-3  查看连接端口
```
    netstat -an | grep EST
```






参考：https://www.cnblogs.com/ftl1012/p/ssh.html