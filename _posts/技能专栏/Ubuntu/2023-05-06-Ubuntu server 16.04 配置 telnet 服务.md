---
layout: post
title: Ubuntu server 16.04 配置 telnet 服务
date: '2023-05-06'
description: 'Ubuntu server 16.04 配置 telnet 服务'
categories: ['【👉👉👉 技能专栏】','【Ubuntu】']
tags: [技能,Ubuntu]
---

##### 1.  首先介绍linux中的守护进程

- 在Linux系统中有一个特殊的守护进程inetd(InterNET services Daemon)，它用于Internet标准服务，通常在系统启动时启动。通过命令行可以给出inetd的配置文件，该配置文件列出了inetd所提供的服务清单。如果没有在命令行给出配置文件，那么inetd将从文件/etc/inetd.conf中读取它的配置信息。inetd的主要任务是为那些没有在系统初始化时启动的服务器进程监听请求，它在同配置文件中列出的服务相关联的TCP或UDP端口上监听请求，当有请求到达这些协议端口时，inetd启动相应的服务器进程。 当一个请求到达由inetd管理的服务端口，inetd将该请求转发给名为 tcpd的程序。tcpd根据配置文件host.｛allow，deny｝来判断是否允许服务该请求。如果请求被允许刚相应的服务器程序(如：ftpd、 telnet)将被启动。这个机制也被称为TCP_Wrapper。
- xinetd（eXended InterNET services Daemon）提供类似于inetd+tcp_wrapper的功能，但是更加强大和安全。在红旗等主流Linux发布商的商业系统中已经逐渐用xinetd取代了inetd，并且提供了访问控制、加强的日志和资源管理功能，成了Linux系统的Internet标准超级守护进程。很多系统服务都用到了xinetd如：FTP、IMAP、POP和telnet等。/etc/services中所有的服务通过他们的端口来访问服务器的时候，先由xinetd来处理，在唤起服务请求之前，xinetd先检验请求者是否满足配置文件中指定的访问控制规则，当前的访问是否超过了指定的同时访问数目，还有配置文件中指定的其他规则等，检查通过，xinetd将这个请求交付到相应的服务去处理，自己就进入sleep状态，等待下一个请求的处理



##### 2. 安装openbsd-inetd

```bash
 apt-get install openbsd-inetd
```



##### 3. 安装telnetd

```bash
 sudo apt-get install xinetd telnetd
```



##### 4. sudo vim /etc/inetd.conf 最后加入以下一行：

```bash
telnet stream tcp nowait telnetd /usr/sbin/tcpd /usr/sbin/in.telnetd
```



##### 5. sudo vim /etc/xinetd.conf并加入以下内容：

```bash
# Simple configuration file for xinetd
# Some defaults, and include /etc/xinetd.d/
defaults
{
# Please note that you need a log_type line to be able to use log_on_success
# and log_on_failure. The default is the following :
# log_type = SYSLOG daemon info
instances = 60
log_type = SYSLOG authpriv
log_on_success = HOST PID
log_on_failure = HOST
cps = 25 30
}
includedir /etc/xinetd.d
```



##### 6. sudo vim /etc/xinetd.d/telnet并加入以下内容：

```bash
# default: on
# description: The telnet server serves telnet sessions;it uses
# unencrypted username/password pairs for authentication.
service telnet
{
disable = no
flags = REUSE
socket_type = stream
wait = no
user = root
server = /usr/sbin/in.telnetd
log_on_failure += USERID
}
```



##### 7. 重启机器或重启网络服务

```bash
sudo /etc/init.d/xinetd restart
```



##### 8. 测试配置是否成功(能通过telent服务器登陆到Ubuntu则成功)

```bash
方法一:  使用TELNET客户端远程(putty登陆工具等)登录
方法二: XP的dos(即开始→运行→cmd)下，输入telnet,然后 open Ubuntu的IP地址(例如：open 192.168.7.106)
 
yexiang@ubuntu:<~>$ telnet 10.0.4.196
Trying 10.0.4.196...
Connected to 10.0.4.196.
Escape character is '^]'.
Ubuntu 16.04.1 LTS
ubuntu login: yexiang
Password: 
Last login: Mon Sep 24 20:46:44 PDT 2018 from 10.0.4.183 on pts/0
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-134-generic x86_64)
```

<div align=left>
<img src="@/../../../../assets/img/Skill_Category/Ubuntu/telnet.jfif" alt="img" />
</div>
