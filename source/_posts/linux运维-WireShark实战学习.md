---
title: 'linux运维: WireShark实战学习'
copyright: true
tags: wireshark
categories: wireshark
abbrlink: 7ebff4
date: 2019-10-06 15:38:02
---
WireShark
<!--more-->

**Capture Filters**

1. protocol name可选值：ether,fddi,ip,arp,rarp,decnet,lat,tcp。 默认为全部协议  

2. direction可选值：src,dst,src and dst, src or dst。默认为src or dst


3. host(s)可选值： net,port,host,portrange。默认为host， 如src 110.119.112.114等价于src host 110.119.112.114

4. Logical可选值： not ,and ,or;
   运算优先级：not具有最高优先级，or和and具有相同优先级；
   运算顺序： 从左至右


**display filters**
* tcp.port==80 只展示端口为80的tcp数据
* !arp     不展示arp协议的数据
* ip.addr==192.168.1.11 只展示地址为192.168.1.111的数据。

> 地址解析协议，即ARP（Address Resolution Protocol），是根据IP地址获取物理地址的一个TCP/IP协议。主机发送信息时将包含目标IP地址的ARP请求广播到局域网络上的所有主机，并接收返回消息，以此确定目标的物理地址；收到返回消息后将该IP地址和物理地址存入本机ARP缓存中并保留一定时间，下次请求时直接查询ARP缓存以节约资源。地址解析协议是建立在网络中各个主机互相信任的基础上的，局域网络上的主机可以自主发送ARP应答消息，其他主机收到应答报文时不会检测该报文的真实性就会将其记入本机ARP缓存；由此攻击者就可以向某一主机发送伪ARP应答报文，使其发送的信息无法到达预期的主机或到达错误的主机，这就构成了一个ARP欺骗。ARP命令可用于查询本机ARP缓存中IP地址和MAC地址的对应关系、添加或删除静态对应关系等。相关协议有RARP、代理ARP。NDP用于在IPv6中代替地址解析协议。

> ICMP（Internet Control Message protocol）互联网信息控制协议，用于实现链路连通性测试和链路追踪，可以实现链路差错报告，属于UDP协议。ICMP有多种类型的报文，同一类型的报文会有多个不同的Code.
ping、tracert 等命令的内部就是用的 icmp 协议


> DNS域名系统（英语：Domain Name System，缩写：DNS）是互联网的一项服务。它作为将域名和IP地址相互映射的一个分布式数据库，能够使人更方便地访问互联网。DNS使用TCP和UDP端口。当前，对于每一级域名长度的限制是63个字符，域名总长度则不能超过253个字符。

> TCP：每次握手时，发送的数据为TCP报文， TCP段包含了源/目的地址、端口号、初始序号、滑动窗口、窗口扩大因子、最大报文段长度等。还有一些标志位：
>* SYN:同步序号
>* ACK：应答回复
>* RST：复位连接，消除旧有的同步序号
>* PSH：尽可能的将数据送往接收进程
>* FIN：发送方完成数据发送
