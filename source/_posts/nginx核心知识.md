---
title: nginx核心知识
tags: nginx
categories: nginx
abbrlink: 9015eb1
date: 2019-08-01 14:07:11
---
nginx学习入门
<!--more-->
### Nginx的三个主要应用场景
* 静态资源服务：通过本地文件系统提供服务
* 反向代理服务：Nginx的强大性能、缓存、负载均衡
* API服务：OpenResty

### Nginx的主要优点
1. 高并发、高性能
2. 可扩展性好
3. 高可靠性
4. 热部署
5. BSD许可证

### Nginx的组成
1. Nginx二进制可执行文件：由各模块源码编译出的一个文件
2. Nginx.conf配置文件：控制Nginx的行为
3. access.log访问日志：记录每一条http请求信息
4. error.log错误日志：定位问题

### Nginx配置语法
1. 配置文件由指令和指令块构成
2. 每条指令以；分号结尾，指令与参数间以空格符号分隔。
3. 使用$符号使用变量
4. 使用#符号添加注释
5. 指令块以{}大括号将多条指令组织在一起
6. include语句允许组合多个配置文件以提高可维护性
7. 部分指令的参数支持正则表达式

### Nginx语法示例
```
http{
	include mime.types;
	upstream thwp{
		server 127.0.0.1:8000;
	}
	server{
		listen 443 http2;
		#Nginx配置语法
		limit_req_zone $binary_remote_addr Zone=one:10m rate=1r/s;
		location ~* \.(gir|jpg|jpeg)$ {
			proxy_cache my_cache;
			expires 3m;proxy_cache_key $host$uri$is_args$args;
			proxy_cache_valid 200 304 302 1d;
			proxy_pass http://javastar.club;
		}
	}
}
```
### 配置参数
__时间的单位__
```
ms:milliseconds
d:days
s:seconds
w:weeks
m:minutes
h:hours
M:months,30days
y:years,365days
```
__空间的单位__
```
    bytes
k/K:kilobytes
m/M:megabytes
g/G:gigabytes
```
### Nginx命令行
1. 格式：nginx -s reload
2. 帮助：-? -h
3. 使用指定的配置文件： -c
4. 指定配置指令：-g
5. 指定运行目录：-p
6. 发送信号：-s (立刻停止服务： stop 优雅的停止服务： quit  重载配置文件： reload  重新开始记录日志文件：reopen)
7. 测试配置文件是否有语法错误：-t -T
8. 打印nginx的版本信息、编译信息等：-v -V

### SSL(Secure Sockets Layer) TLS(Transport Layer Security)
__ISO/OSI模型__
应用层
表示层
会话层
传输层
网络层
数据链路层
物理层
__TCP/IP模型__
应用层
传输层
网络层
链路层
__TLS安全密码套件__

TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256

ECDHE:密钥交换算法
RSA：身份验证算法
AES_128_GCM:对称加密算法、强度、分组模式
SHA256:签名hash算法

### PKI公钥基础设施
证书签名申请(request certificate issuance,CSR)
证书类型：
域名验证(domain validated,DV)证书
组织验证(organization validated,OV)证书
扩展验证(extended validation,EV)证书
证书链

### TLS通讯过程
1. 验证身份
2. 达成安全套件共识
3. 传递密钥
4. 加密通讯

### Nginx简介
Nginx 是一款轻量级的 Web （HTTP）服务器/反向代理服务器及电子邮件（IMAP/POP3）代理服务器
优势：能支持高达 50,000 个并发连接数  ；支持热部署  ；很高的稳定性（抵御dos攻击）

 

### 架构 
 在 unix 系统中会以 daemon （守护进程）的方式在后台运行，后台进程包含一个 master 进程和多个 worker 进程（多进程的工作方式）
 1、多个 worker 进程之间是对等的，他们同等竞争来自客户端的请求，各进程互相之间是独立的。

2、一个请求，只可能在一个 worker 进程中处理，一个 worker 进程，不可能处理其它进程的请求。

3、推荐设置 worker 的个数为 cpu 的核数

4、异步非阻塞 （非阻塞不会让出cpu导致切换浪费）

 

### 相关概念

__1、connection__

是对 tcp 连接的封装;

Nginx 通过设置 worker_connectons 来设置每个worker进程支持的最大连接数;

Nginx 能建立的最大连接数，应该是worker_connections * worker_processes;对于 HTTP 请求本地资源来说，能够支持的最大并发数量是worker_connections * worker_processes，而如果是 HTTP 作为反向代理来说，最大并发数量应该是worker_connections * worker_processes/2。因为作为反向代理服务器，每个并发会建立与客户端的连接和与后端服务的连接，会占用两个连接。

__2、request__

Nginx 中指 http 请求;

web服务器工作流：http 请求是典型的请求-响应类型的的网络协议，而 http 是文本协议，所以我们在分析请求行与请求头，以及输出响应行与响应头，往往是一行一行的进行处理。如果我们自己来写一个 http 服务器，通常在一个连接建立好后，客户端会发送请求过来。然后我们读取一行数据，分析出请求行中包含的 method、uri、http_version 信息。然后再一行一行处理请求头，并根据请求 method 与请求头的信息来决定是否有请求体以及请求体的长度，然后再去读取请求体。得到请求后，我们处理请求产生需要输出的数据，然后再生成响应行，响应头以及响应体。在将响应发送给客户端之后，一个完整的请求就处理完了。

__3、keepalive__

长连接: http 请求是基于 TCP 协议之上的，那么，当客户端在发起请求前，需要先与服务端建立 TCP 连接(三次握手)，当连接断开后（四次挥手）。而 http 请求是请求应答式的，如果我们能知道每个请求头与响应体的长度，那么我们是可以在一个连接上面执行多个请求的，这就是所谓的长连接，但前提条件是我们先得确定请求头与响应体的长度。对于请求来说，如果当前请求需要有body，如 POST 请求，那么 Nginx 就需要客户端在请求头中指定 content-length 来表明 body 的大小，否则返回 400 错误。也就是说，请求体的长度是确定的，那么响应体的长度呢？先来看看 http 协议中关于响应 body 长度的确定：
```
    对于 http1.0 协议来说，如果响应头中有 content-length 头，则以 content-length 的长度就可以知道 body 的长度了，客户端在接收 body 时，就可以依照这个长度来接收数据，接收完后，就表示这个请求完成了。而如果没有 content-length 头，则客户端会一直接收数据，直到服务端主动断开连接，才表示 body 接收完了。
    而对于 http1.1 协议来说，如果响应头中的 Transfer-encoding 为 chunked 传输，则表示 body 是流式输出，body 会被分成多个块，每块的开始会标识出当前块的长度，此时，body 不需要通过长度来指定。如果是非 chunked 传输，而且有 content-length，则按照 content-length 来接收数据。否则，如果是非 chunked，并且没有 content-length，则客户端接收数据，直到服务端主动断开连接。
```
从上面，我们可以看到，除了 http1.0 不带 content-length 以及 http1.1 非 chunked 不带 content-length 外，body 的长度是可知的。此时，当服务端在输出完 body 之后，会可以考虑使用长连接。能否使用长连接，也是有条件限制的。如果客户端的请求头中的 connection为close，则表示客户端需要关掉长连接，如果为 keep-alive，则客户端需要打开长连接，如果客户端的请求中没有 connection 这个头，那么根据协议，如果是 http1.0，则默认为 close，如果是 http1.1，则默认为 keep-alive。如果结果为 keepalive，那么，Nginx 在输出完响应体后，会设置当前连接的 keepalive 属性，然后等待客户端下一次请求。当然，Nginx 不可能一直等待下去，如果客户端一直不发数据过来，岂不是一直占用这个连接？所以当 Nginx 设置了 keepalive 等待下一次的请求时，同时也会设置一个最大等待时间，这个时间是通过选项 keepalive_timeout 来配置的，如果配置为 0，则表示关掉 keepalive，此时，http 版本无论是 1.1 还是 1.0，客户端的 connection 不管是 close 还是 keepalive，都会强制为 close。

如果服务端最后的决定是 keepalive 打开，那么在响应的 http 头里面，也会包含有 connection 头域，其值是"Keep-Alive"，否则就是"Close"。如果 connection 值为 close，那么在 Nginx 响应完数据后，会主动关掉连接。所以，对于请求量比较大的 Nginx 来说，关掉 keepalive 最后会产生比较多的 time-wait 状态的 socket。一般来说，当客户端的一次访问，需要多次访问同一个 server 时，打开 keepalive 的优势非常大，比如图片服务器，通常一个网页会包含很多个图片。打开 keepalive 也会大量减少 time-wait 的数量。

__4、pipe__

http1.1 引入新特性,keepalive 的一种升华，基于长连接的，目的就是利用一个连接做多次请求;

对 pipeline 来说，客户端不必等到第一个请求处理完后，就可以马上发起第二个请求;

 

__5、linger_close__

延迟关闭，也就是说，当 Nginx 要关闭连接时，并非立即关闭连接，而是先关闭 tcp 连接的写，再等待一段时间后再关掉连接的读。

 

### 四、配置

__1、nginx.conf__

指令上下文：

    main: Nginx 在运行时与具体业务功能（比如http服务或者email服务代理）无关的一些参数，比如工作进程数，运行的身份等。
    http: 与提供 http 服务相关的一些配置参数。例如：是否使用 keepalive 啊，是否使用gzip进行压缩等。
    server: http 服务上支持若干虚拟主机。每个虚拟主机一个对应的 server 配置项，配置项里面包含该虚拟主机相关的配置。在提供 mail 服务的代理时，也可以建立若干 server，每个 server 通过监听的地址来区分。
    location: http 服务中，某些特定的URL对应的一系列配置项。
    mail: 实现 email 相关的 SMTP/IMAP/POP3 代理时，共享的一些配置项（因为可能实现多个代理，工作在多个监听地址上）。

 

示例：
```
worker_processes 1;      //一般设置为cpu核数

error_log logs/error.log error;
pid logs/nginx.pid;

events {
　　worker_connections 1024;     //每个worker的最大连接数
}

http {
　　include mime.types;
　　default_type application/octet-stream;
　　sendfile on;
　　tcp_nopush on;
　　server_names_hash_bucket_size 128;
　　keepalive_timeout 1800s;      //支持长连接

　　client_max_body_size 0;

　　proxy_connect_timeout 5s;
　　proxy_read_timeout 1800s;

　　proxy_set_header X-Real-IP $remote_addr;
　　proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
　　proxy_set_header Host $http_host;

 

 

     upstream web_vmaxfmproxy_pool { server 10.43.136.220:27430; }
     upstream web_vmaxdatacheck_pool { server 10.43.136.220:27340; }

　　server {
　　　　listen 28888;
           server_name web_web_pool;
　　　　location ~ ^/web/cometd {
                     proxy_pass http://web_web_pool;
                     proxy_http_version 1.1;
                     proxy_set_header Upgrade $http_upgrade;
                     proxy_set_header Connection "upgrade";
　　　　}

　　     location / {
　　　　　　if ($uri ~ ^/vmaxfmproxy/){ proxy_pass http://web_vmaxfmproxy_pool; break; }
　　　　　　if ($uri ~ ^/vmaxdatacheck/){ proxy_pass http://web_vmaxdatacheck_pool; break; }
　　　　}

　　　　location /rdk/service {
　　　　　　proxy_pass http://localhost:5555;
　　　　}
　　　　location ~ /rdk/app/(?<section>.*) {
　　　　　　proxy_pass http://web_rdk_server_pool/rdk_server/app/$section;
　　　　}

　　　　location /web/res/web-framework/default.html {
　　　　　　rewrite /web/res/web-framework/default.html /rdk/app/portal/web/index.html permanent;
　　　　}

}

}
```
 
### 五、nginx模块

    event module: 搭建了独立于操作系统的事件处理机制的框架，及提供了各具体事件的处理。包括 ngx_events_module， ngx_event_core_module和ngx_epoll_module 等。Nginx 具体使用何种事件处理模块，这依赖于具体的操作系统和编译选项。

    phase handler: 此类型的模块也被直接称为 handler 模块。主要负责处理客户端请求并产生待响应内容，比如 ngx_http_static_module 模块，负责客户端的静态页面请求处理并将对应的磁盘文件准备为响应内容输出。

    output filter: 也称为 filter 模块，主要是负责对输出的内容进行处理，可以对输出进行修改。例如，可以实现对输出的所有 html 页面增加预定义的 footbar 一类的工作，或者对输出的图片的 URL 进行替换之类的工作。

    upstream: upstream 模块实现反向代理的功能，将真正的请求转发到后端服务器上，并从后端服务器上读取响应，发回客户端。upstream 模块是一种特殊的 handler，只不过响应内容不是真正由自己产生的，而是从后端服务器上读取的。
    load-balancer: 负载均衡模块，实现特定的算法，在众多的后端服务器中，选择一个服务器出来作为某个请求的转发服务器。

 

 

### 六、nginx 请求过程

所有实际上的业务处理逻辑都在 worker 进程。worker 进程中有一个函数，执行无限循环，不断处理收到的来自客户端的请求，并进行处理，直到整个 Nginx 服务被停止。

worker 进程中，ngx_worker_process_cycle()函数就是这个无限循环的处理函数。在这个函数中，一个请求的简单处理流程如下：

    操作系统提供的机制（例如 epoll, kqueue 等）产生相关的事件。
    接收和处理这些事件，如是接受到数据，则产生更高层的 request 对象。
    处理 request 的 header 和 body。
    产生响应，并发送回客户端。
    完成 request 的处理。
    重新初始化定时器及其他事件。

为了让大家更好的了解 Nginx 中请求处理过程，我们以 HTTP Request 为例，来做一下详细地说明。

从 Nginx 的内部来看，一个 HTTP Request 的处理过程涉及到以下几个阶段。

    初始化 HTTP Request（读取来自客户端的数据，生成 HTTP Request 对象，该对象含有该请求所有的信息）。
    处理请求头。
    处理请求体。
    如果有的话，调用与此请求（URL 或者 Location）关联的 handler。
    依次调用各 phase handler 进行处理。

在这里，我们需要了解一下 phase handler 这个概念。phase 字面的意思，就是阶段。所以 phase handlers 也就好理解了，就是包含若干个处理阶段的一些 handler。

在每一个阶段，包含有若干个 handler，再处理到某个阶段的时候，依次调用该阶段的 handler 对 HTTP Request 进行处理。

通常情况下，一个 phase handler 对这个 request 进行处理，并产生一些输出。通常 phase handler 是与定义在配置文件中的某个 location 相关联的。

一个 phase handler 通常执行以下几项任务：

    获取 location 配置。
    产生适当的响应。
    发送 response header。
    发送 response body。

当 Nginx 读取到一个 HTTP Request 的 header 的时候，Nginx 首先查找与这个请求关联的虚拟主机的配置。如果找到了这个虚拟主机的配置，那么通常情况下，这个 HTTP Request 将会经过以下几个阶段的处理（phase handlers）：
```
    NGX_HTTP_POST_READ_PHASE: 读取请求内容阶段
    NGX_HTTP_SERVER_REWRITE_PHASE: Server 请求地址重写阶段
    NGX_HTTP_FIND_CONFIG_PHASE: 配置查找阶段:
    NGX_HTTP_REWRITE_PHASE: Location请求地址重写阶段
    NGX_HTTP_POST_REWRITE_PHASE: 请求地址重写提交阶段
    NGX_HTTP_PREACCESS_PHASE: 访问权限检查准备阶段
    NGX_HTTP_ACCESS_PHASE: 访问权限检查阶段
    NGX_HTTP_POST_ACCESS_PHASE: 访问权限检查提交阶段
    NGX_HTTP_TRY_FILES_PHASE: 配置项 try_files 处理阶段
    NGX_HTTP_CONTENT_PHASE: 内容产生阶段
    NGX_HTTP_LOG_PHASE: 日志模块处理阶段
```
在内容产生阶段，为了给一个 request 产生正确的响应，Nginx 必须把这个 request 交给一个合适的 content handler 去处理。如果这个 request 对应的 location 在配置文件中被明确指定了一个 content handler，那么Nginx 就可以通过对 location 的匹配，直接找到这个对应的 handler，并把这个 request 交给这个 content handler 去处理。这样的配置指令包括像，perl，flv，proxy_pass，mp4等。

如果一个 request 对应的 location 并没有直接有配置的 content handler，那么 Nginx 依次尝试:

    如果一个 location 里面有配置 random_index on，那么随机选择一个文件，发送给客户端。
    如果一个 location 里面有配置 index 指令，那么发送 index 指令指明的文件，给客户端。
    如果一个 location 里面有配置 autoindex on，那么就发送请求地址对应的服务端路径下的文件列表给客户端。
    如果这个 request 对应的 location 上有设置 gzip_static on，那么就查找是否有对应的.gz文件存在，有的话，就发送这个给客户端（客户端支持 gzip 的情况下）。
    请求的 URI 如果对应一个静态文件，static module 就发送静态文件的内容到客户端。

内容产生阶段完成以后，生成的输出会被传递到 filter 模块去进行处理。filter 模块也是与 location 相关的。所有的 fiter 模块都被组织成一条链。输出会依次穿越所有的 filter，直到有一个 filter 模块的返回值表明已经处理完成。

这里列举几个常见的 filter 模块，例如：
```
    server-side includes。
    XSLT filtering。
    图像缩放之类的。
    gzip 压缩。
```
在所有的 filter 中，有几个 filter 模块需要关注一下。按照调用的顺序依次说明如下：
```
    write: 写输出到客户端，实际上是写到连接对应的 socket 上。
    postpone: 这个 filter 是负责 subrequest 的，也就是子请求的。
    copy: 将一些需要复制的 buf(文件或者内存)重新复制一份然后交给剩余的 body filter 处理。
```

### 七、upstream模块

__1、upstream 模块__

将使 Nginx 跨越单机的限制，完成网络数据的接收、处理和转发。

数据转发功能，为 Nginx 提供了跨越单机的横向处理能力，使 Nginx 摆脱只能为终端节点提供单一功能的限制，而使它具备了网路应用级别的拆分、封装和整合的战略功能。
在云模型大行其道的今天，数据转发是 Nginx 有能力构建一个网络应用的关键组件。

upstream 属于 handler，只是他不产生自己的内容，而是通过请求后端服务器得到内容，所以才称为 upstream（上游）。
请求并取得响应内容的整个过程已经被封装到 Nginx 内部，所以 upstream 模块只需要开发若干回调函数，完成构造请求和解析响应等具体的工作。

__2、负载均衡模块__

负载均衡模块用于从upstream指令定义的后端主机列表中选取一台主机。

Nginx 先使用负载均衡模块找到一台主机，再使用 upstream 模块实现与这台主机的交互。

如果需要使用 ip hash 的负载均衡算法：

示例 :

```
worker_processes  1;
error_log  logs/error.log  error;
pid        logs/nginx.pid;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;   
    sendfile       on;
    tcp_nopush     on;
    server_names_hash_bucket_size 128;  
    keepalive_timeout  600s;
    proxy_connect_timeout 5s;
    proxy_read_timeout 600s;
    
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;

    upstream lw_web_rdk_server_pool {
         server  10.43.149.160:26180  weight=1;

         server  10.43.136.220:5812  weight=1;
      }

    server {
        listen  26188;
        server_name localhost;
        location /{
        proxy_pass  http://lw_web_rdk_server_pool;
        proxy_redirect default;
       }
    }
    server {
       listen  26185;
       server_name localhost;
       location /rdk/service {
                            proxy_pass http://localhost:5812;
                    }
       location / {
        root   ../../;
        index  index.html index.htm;
       }
    }

}
```

## Nginx源码剖析，本次代码分析主要基于nginx-1.17.2版本
Nginx的源码主要分布在src/目录下，而src/目录下主要包含三部分比较重要的模块。

1. core：包含了Nginx的最基础的库和框架。包括了内存池、链表、hashmap、String等常用的数据结构。

2. event：事件模块。Nginx自己实现了事件模型。而我们所熟悉的Memcached是使用了Libevent的事件库。自己实现event会性能和效率方便更加高效。

3. http：实现HTTP的模块。实现了HTTP的具体协议的各种模块，该部分内容量比较大。


### Nginx架构

Nginx是一款多进程的软件。Nginx启动后，会产生一个master进程和N个工作进程。其中nginx.conf中可以配置工作进程的个数：
```
worker_processes  1;
```
多进程模块有一个非常大的好处，就是不需要太多考虑并发锁的问题。

Nginx的内存管理是通过内存池来实现的。Nginx的内存池的设计非常的精巧，很多场景下，我们可以将Nginx的内存池实现抽象出来改造成我们开发中的内存池。


### 内存池

一般我们使用malloc/alloc/free等函数来分配和释放内存。但是直接使用这些函数会有一些弊端：

1. 虽然系统自带的ptmalloc内存分配管理器，也有自己的内存优化管理方案（申请内存块以及将内存交还给系统都有自己的优化方案，具体可以研究一下ptmalloc的源码），但是直接使用malloc/alloc/free，仍然会导致内存分配的性能比较低。

2. 频繁使用这些函数分配和释放内存，会导致内存碎片，不容易让系统直接回收内存。典型的例子就是大并发频繁分配和回收内存，会导致进程的内存产生碎片，并且不会立马被系统回收。

3. 容易产生内存泄露。


使用内存池分配内存有几点好处：

1. 提升内存分配效率。不需要每次分配内存都执行malloc/alloc等函数。

2. 让内存的管理变得更加简单。内存的分配都会在一块大的内存上，回收的时候只需要回收大块内存就能将所有的内存回收，防止了内存管理混乱和内存泄露问题。

### 数据结构定义
先看ngx_palloc.h里面的结构体定义：
__ngx_pool_t 内存池主结构__
```
/**
 * Nginx 内存池数据结构
 */
struct ngx_pool_s {
    ngx_pool_data_t       d;        /* 内存池的数据区域*/
    size_t                max;      /* 最大每次可分配内存 */
    ngx_pool_t           *current;  /* 指向当前的内存池指针地址。ngx_pool_t链表上最后一个缓存池结构*/
    ngx_chain_t          *chain;    /* 缓冲区链表 */
    ngx_pool_large_t     *large;    /* 存储大数据的链表 */
    ngx_pool_cleanup_t   *cleanup;  /* 可自定义回调函数，清除内存块分配的内存 */
    ngx_log_t            *log;      /* 日志 */
};
```
__ngx_pool_data_t 数据区域结构__
```
typedef struct {
    u_char               *last;  /* 内存池中未使用内存的开始节点地址 */
    u_char               *end;   /* 内存池的结束地址 */
    ngx_pool_t           *next;  /* 指向下一个内存池 */
    ngx_uint_t            failed;/* 失败次数 */
} ngx_pool_data_t;
```
__ngx_pool_large_t 大数据块结构__
```
struct ngx_pool_large_s {
    ngx_pool_large_t     *next;   /* 指向下一个存储地址 通过这个地址可以知道当前块长度 */
    void                 *alloc;  /* 数据块指针地址 */
};
```
__ngx_pool_cleanup_t 自定义清理回调的数据结构__
```
struct ngx_pool_cleanup_s {
    ngx_pool_cleanup_pt   handler;  /* 清理的回调函数 */
    void                 *data;     /* 指向存储的数据地址 */
    ngx_pool_cleanup_t   *next;     /* 下一个ngx_pool_cleanup_t */
};
```

1. Nginx的内存池会放在ngx_pool_t的数据结构上（ngx_pool_data_t用于记录内存块block的可用地址空间和内存块尾部）。当初始化分配的内存块大小不能满足需求的时候，Nginx就会调用ngx_palloc_block函数来分配一个新的内存块，通过链表的形式连接起来。

2. 当申请的内存大于pool->max的值的时候，Nginx就会单独分配一块large的内存块，会放置在pool->large的链表结构上。

3. pool->cleanup的链表结构主要存放需要通过回调函数清理的内存数据。（例如文件描述符）

### 创建内存池ngx_create_pool

调用ngx_create_pool这个方法就可以创建一个内存池。
```
/**
 * 创建一个内存池
 */
ngx_pool_t *
ngx_create_pool(size_t size, ngx_log_t *log) {
    ngx_pool_t *p;
 
    /**
     * 相当于分配一块内存 ngx_alloc(size, log)
     */
    p = ngx_memalign(NGX_POOL_ALIGNMENT, size, log);
    if (p == NULL) {
        return NULL;
    }
 
    /**
     * Nginx会分配一块大内存，其中内存头部存放ngx_pool_t本身内存池的数据结构
     * ngx_pool_data_t  p->d 存放内存池的数据部分（适合小于p->max的内存块存储）
     * p->large 存放大内存块列表
     * p->cleanup 存放可以被回调函数清理的内存块（该内存块不一定会在内存池上面分配）
     */
    p->d.last = (u_char *) p + sizeof(ngx_pool_t); //内存开始地址，指向ngx_pool_t结构体之后数据取起始位置
    p->d.end = (u_char *) p + size; //内存结束地址
    p->d.next = NULL; //下一个ngx_pool_t 内存池地址
    p->d.failed = 0; //失败次数
 
    size = size - sizeof(ngx_pool_t);
    p->max = (size < NGX_MAX_ALLOC_FROM_POOL) ? size : NGX_MAX_ALLOC_FROM_POOL;
 
    /* 只有缓存池的父节点，才会用到下面的这些  ，子节点只挂载在p->d.next,并且只负责p->d的数据内容*/
    p->current = p;
    p->chain = NULL;
    p->large = NULL;
    p->cleanup = NULL;
    p->log = log;
 
    return p;
}
```

### 销毁内存池ngx_destroy_pool
```
/**
 * 销毁内存池。
 */
void ngx_destroy_pool(ngx_pool_t *pool) {
    ngx_pool_t *p, *n;
    ngx_pool_large_t *l;
    ngx_pool_cleanup_t *c;
 
    /* 首先清理pool->cleanup链表 */
    for (c = pool->cleanup; c; c = c->next) {
        /* handler 为一个清理的回调函数 */
        if (c->handler) {
            ngx_log_debug1(NGX_LOG_DEBUG_ALLOC, pool->log, 0,
                    "run cleanup: %p", c);
            c->handler(c->data);
        }
    }
 
    /* 清理pool->large链表（pool->large为单独的大数据内存块）  */
    for (l = pool->large; l; l = l->next) {
 
        ngx_log_debug1(NGX_LOG_DEBUG_ALLOC, pool->log, 0, "free: %p", l->alloc);
 
        if (l->alloc) {
            ngx_free(l->alloc);
        }
    }
 
#if (NGX_DEBUG)
 
    /*
     * we could allocate the pool->log from this pool
     * so we cannot use this log while free()ing the pool
     */
 
    for (p = pool, n = pool->d.next; /* void */; p = n, n = n->d.next) {
        ngx_log_debug2(NGX_LOG_DEBUG_ALLOC, pool->log, 0,
                "free: %p, unused: %uz", p, p->d.end - p->d.last);
 
        if (n == NULL) {
            break;
        }
    }
 
#endif
 
    /* 对内存池的data数据区域进行释放 */
    for (p = pool, n = pool->d.next; /* void */; p = n, n = n->d.next) {
        ngx_free(p);
 
        if (n == NULL) {
            break;
        }
    }
}
```
### 重设内存池ngx_reset_pool
```
/**
 * 重设内存池
 */
void ngx_reset_pool(ngx_pool_t *pool) {
    ngx_pool_t *p;
    ngx_pool_large_t *l;
 
    /* 清理pool->large链表（pool->large为单独的大数据内存块）  */
    for (l = pool->large; l; l = l->next) {
        if (l->alloc) {
            ngx_free(l->alloc);
        }
    }
 
    pool->large = NULL;
 
    /* 循环重新设置内存池data区域的 p->d.last；data区域数据并不擦除*/
    for (p = pool; p; p = p->d.next) {
        p->d.last = (u_char *) p + sizeof(ngx_pool_t);
    }
}
```
### 使用内存池分配一块内存ngx_palloc和ngx_pnalloc
```
/**
 * 内存池分配一块内存，返回void类型指针
 */
void *
ngx_palloc(ngx_pool_t *pool, size_t size) {
    u_char *m;
    ngx_pool_t *p;
 
    /* 判断每次分配的内存大小，如果超出pool->max的限制，则需要走大数据内存分配策略 */
    if (size <= pool->max) {
 
        p = pool->current;
 
        /*
         * 循环读取缓存池链p->d.next的各个的ngx_pool_t节点，
         * 如果剩余的空间可以容纳size，则返回指针地址
         *
         * 这边的循环，实际上最多只有4次，具体可以看ngx_palloc_block函数
         * */
        do {
            /* 对齐操作,会损失内存，但是提高内存使用速度 */
            m = ngx_align_ptr(p->d.last, NGX_ALIGNMENT);
 
            if ((size_t)(p->d.end - m) >= size) {
                p->d.last = m + size;
 
                return m;
            }
 
            p = p->d.next;
 
        } while (p);
 
        /* 如果没有缓存池空间没有可以容纳大小为size的内存块，则需要重新申请一个缓存池pool节点 */
        return ngx_palloc_block(pool, size);
    }
 
    /* 走大数据分配策略 ，在pool->large链表上分配 */
    return ngx_palloc_large(pool, size);
}
 
/**
 * 内存池分配一块内存，返回void类型指针
 * 不考虑对齐情况
 */
void *
ngx_pnalloc(ngx_pool_t *pool, size_t size) {
    u_char *m;
    ngx_pool_t *p;
 
    /* 判断每次分配的内存大小，如果超出pool->max的限制，则需要走大数据内存分配策略 */
    if (size <= pool->max) {
 
        p = pool->current;
 
        /* 循环读取数据区域的各个ngx_pool_t缓存池链，如果剩余的空间可以容纳size，则返回指针地址*/
        do {
            m = p->d.last; //分配的内存块的地址
 
            if ((size_t)(p->d.end - m) >= size) {
                p->d.last = m + size;
 
                return m;
            }
 
            p = p->d.next;
 
        } while (p);
 
        /* 如果没有缓存池空间没有可以容纳大小为size的内存块，则需要重新申请一个缓存池*/
        return ngx_palloc_block(pool, size);
    }
 
    /* 走大数据分配策略 */
    return ngx_palloc_large(pool, size);
}
```
__内存分配逻辑:__

1. 分配一块内存，如果分配的内存size小于内存池的pool->max的限制，则属于小内存块分配，走小内存块分配逻辑；否则走大内存分配逻辑。

2. 小内存分配逻辑：循环读取pool->d上的内存块，是否有足够的空间容纳需要分配的size，如果可以容纳，则直接分配内存；否则内存池需要申请新的内存块，调用ngx_palloc_block。

3. 大内存分配逻辑：当分配的内存size大于内存池的pool->max的限制，则会直接调用ngx_palloc_large方法申请一块独立的内存块，并且将内存块挂载到pool->large的链表上进行统一管理。

ngx_palloc_block，内存池扩容：
```
/**
 * 申请一个新的缓存池 ngx_pool_t
 * 新的缓存池会挂载在主缓存池的 数据区域 （pool->d->next）
 */
static void *
ngx_palloc_block(ngx_pool_t *pool, size_t size) {
    u_char *m;
    size_t psize;
    ngx_pool_t *p, *new, *current;
 
    psize = (size_t)(pool->d.end - (u_char *) pool);
 
    /* 申请新的块 */
    m = ngx_memalign(NGX_POOL_ALIGNMENT, psize, pool->log);
    if (m == NULL) {
        return NULL;
    }
 
    new = (ngx_pool_t *) m;
 
    new->d.end = m + psize;
    new->d.next = NULL;
    new->d.failed = 0;
 
    /* 分配size大小的内存块，返回m指针地址 */
    m += sizeof(ngx_pool_data_t);
    m = ngx_align_ptr(m, NGX_ALIGNMENT);
    new->d.last = m + size;
 
    current = pool->current;
 
    /**
     * 缓存池的pool数据结构会挂载子节点的ngx_pool_t数据结构
     * 子节点的ngx_pool_t数据结构中只用到pool->d的结构，只保存数据
     * 每添加一个子节点，p->d.failed就会+1，当添加超过4个子节点的时候，
     * pool->current会指向到最新的子节点地址
     *
     * 这个逻辑主要是为了防止pool上的子节点过多，导致每次ngx_palloc循环pool->d.next链表
     * 将pool->current设置成最新的子节点之后，每次最大循环4次，不会去遍历整个缓存池链表
     */
    for (p = current; p->d.next; p = p->d.next) {
        if (p->d.failed++ > 4) {
            current = p->d.next;
        }
    }
 
    p->d.next = new;
 
    /* 最终这个还是没变 */
    pool->current = current ? current : new;
 
    return m;
}
```
分配一块大内存，挂载到pool->large链表上ngx_palloc_large
```
/**
 * 当分配的内存块大小超出pool->max限制的时候,需要分配在pool->large上
 */
static void *
ngx_palloc_large(ngx_pool_t *pool, size_t size) {
    void *p;
    ngx_uint_t n;
    ngx_pool_large_t *large;
 
    /* 分配一块新的大内存块 */
    p = ngx_alloc(size, pool->log);
    if (p == NULL) {
        return NULL;
    }
 
    n = 0;
 
    /* 去pool->large链表上查询是否有NULL的，只在链表上往下查询3次，主要判断大数据块是否有被释放的，如果没有则只能跳出*/
    for (large = pool->large; large; large = large->next) {
        if (large->alloc == NULL) {
            large->alloc = p;
            return p;
        }
 
        if (n++ > 3) {
            break;
        }
    }
 
    /* 分配一个ngx_pool_large_t 数据结构 */
    large = ngx_palloc(pool, sizeof(ngx_pool_large_t));
    if (large == NULL) {
        ngx_free(p); //如果分配失败，删除内存块
        return NULL;
    }
 
    large->alloc = p;
    large->next = pool->large;
    pool->large = large;
 
    return p;
}
```
大内存块的释放ngx_pfree

内存池释放需要走ngx_destroy_pool，独立大内存块的单独释放，可以走ngx_pfree方法。
```
/**
 * 大内存块释放  pool->large
 */
ngx_int_t ngx_pfree(ngx_pool_t *pool, void *p) {
    ngx_pool_large_t *l;
 
    /* 在pool->large链上循环搜索，并且只释放内容区域，不释放ngx_pool_large_t数据结构*/
    for (l = pool->large; l; l = l->next) {
        if (p == l->alloc) {
            ngx_log_debug1(NGX_LOG_DEBUG_ALLOC, pool->log, 0,
                    "free: %p", l->alloc);
            ngx_free(l->alloc);
            l->alloc = NULL;
 
            return NGX_OK;
        }
    }
 
    return NGX_DECLINED;
}
```
cleanup机制 可以回调函数清理数据

Nginx的内存池cleanup机制，设计的非常巧妙。pool->cleanup本身是一个链表，每个ngx_pool_cleanup_t的数据结构上，保存着内存数据的本身cleanup->data和回调清理函数cleanup->handler。

通过cleanup的机制，我们就可以在内存池上保存例如文件句柄fd的资源。当我们调用ngx_destroy_pool方法销毁内存池的时候，首先会来清理pool->cleanup，并且都会执行c->handler(c->data)回调函数，用于清理资源。

Nginx的这个机制，最显著的就是让文件描述符和需要自定义清理的数据的管理变得更加简单。


分配一个cleanup结构：
```
/**
 * 分配一个可以用于回调函数清理内存块的内存
 * 内存块仍旧在p->d或p->large上
 *
 * ngx_pool_t中的cleanup字段管理着一个特殊的链表，该链表的每一项都记录着一个特殊的需要释放的资源。
 * 对于这个链表中每个节点所包含的资源如何去释放，是自说明的。这也就提供了非常大的灵活性。
 * 意味着，ngx_pool_t不仅仅可以管理内存，通过这个机制，也可以管理任何需要释放的资源，
 * 例如，关闭文件，或者删除文件等等的。下面我们看一下这个链表每个节点的类型
 *
 * 一般分两种情况：
 * 1. 文件描述符
 * 2. 外部自定义回调函数可以来清理内存
 */
ngx_pool_cleanup_t *
ngx_pool_cleanup_add(ngx_pool_t *p, size_t size) {
    ngx_pool_cleanup_t *c;
 
    /* 分配一个ngx_pool_cleanup_t */
    c = ngx_palloc(p, sizeof(ngx_pool_cleanup_t));
    if (c == NULL) {
        return NULL;
    }
 
    /* 如果size !=0 从pool->d或pool->large分配一个内存块 */
    if (size) {
        /*  */
        c->data = ngx_palloc(p, size);
        if (c->data == NULL) {
            return NULL;
        }
 
    } else {
        c->data = NULL;
    }
 
    /* handler为回调函数 */
    c->handler = NULL;
    c->next = p->cleanup;
 
    p->cleanup = c;
 
    ngx_log_debug1(NGX_LOG_DEBUG_ALLOC, p->log, 0, "add cleanup: %p", c);
 
    return c;
}
```
手动清理 p->cleanup链表上的数据：（内存池销毁函数ngx_destroy_pool也会清理p->cleanup）
```
/**
 * 清除 p->cleanup链表上的内存块（主要是文件描述符）
 * 回调函数：ngx_pool_cleanup_file
 */
void ngx_pool_run_cleanup_file(ngx_pool_t *p, ngx_fd_t fd) {
    ngx_pool_cleanup_t *c;
    ngx_pool_cleanup_file_t *cf;
 
    for (c = p->cleanup; c; c = c->next) {
        if (c->handler == ngx_pool_cleanup_file) {
 
            cf = c->data;
 
            if (cf->fd == fd) {
                c->handler(cf); /* 调用回调函数 */
                c->handler = NULL;
                return;
            }
        }
    }
}
```

关闭文件的回调函数和删除文件的回调函数。这个是文件句柄通用的回调函数，可以放置在p->cleanup->handler上。 
```
/**
 * 关闭文件回调函数
 * ngx_pool_run_cleanup_file方法执行的时候，用了此函数作为回调函数的，都会被清理
 */
void ngx_pool_cleanup_file(void *data) {
    ngx_pool_cleanup_file_t *c = data;
 
    ngx_log_debug1(NGX_LOG_DEBUG_ALLOC, c->log, 0, "file cleanup: fd:%d",
            c->fd);
 
    if (ngx_close_file(c->fd) == NGX_FILE_ERROR) {
        ngx_log_error(NGX_LOG_ALERT, c->log, ngx_errno,
                ngx_close_file_n " \"%s\" failed", c->name);
    }
}
```

```
/**
 * 删除文件回调函数
 */
void ngx_pool_delete_file(void *data) {
    ngx_pool_cleanup_file_t *c = data;
 
    ngx_err_t err;
 
    ngx_log_debug2(NGX_LOG_DEBUG_ALLOC, c->log, 0, "file cleanup: fd:%d %s",
            c->fd, c->name);
 
    if (ngx_delete_file(c->name) == NGX_FILE_ERROR) {
        err = ngx_errno;
 
        if (err != NGX_ENOENT) {
            ngx_log_error(NGX_LOG_CRIT, c->log, err,
                    ngx_delete_file_n " \"%s\" failed", c->name);
        }
    }
 
    if (ngx_close_file(c->fd) == NGX_FILE_ERROR) {
        ngx_log_error(NGX_LOG_ALERT, c->log, ngx_errno,
                ngx_close_file_n " \"%s\" failed", c->name);
    }
}
```