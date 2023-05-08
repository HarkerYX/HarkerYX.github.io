---
layout: post
title: Ubuntu server 16.04 配置 ftp 服务
date: '2023-05-06'
description: 'Ubuntu server 16.04 配置 ftp 服务'
categories: ['【👉👉👉 技能专栏】','【Ubuntu】']
tags: [技能,Ubuntu]
---

##### 1. 安装 vsftpd

```bash
sudo apt-get install vsftpd
 
安装完毕后，/srv下会增加一个ftp目录。同时系统会增加一个名为ftp的用户组，可以用
~$ sudo cat /etc/shadow 查看， 如 ftp:*:16977:0:99999:7:::
```

##### 2. 启动 vsftpd

```bash
sudo service vsftpd start
```

##### 3. 查看当前所有进程：

```bash
 ~$ ps -e
 
2183 ? 00:00:00 vsftpd
```

- 至此服务器端 vsftp 的最基本配置已完成，vsftpd 已开启（注意你的防火墙配置，作为简单试验可以直接停用防火墙）
- 关闭 vsftpd 进程只需要执行 `sudo service vsftpd stop`，同时还可以使用命令 `pgrep vsftpd ` 来查看进程 vsftp 是否存在

```bash
yexiang@ubuntu:<ftp_share>$ pgrep vsftpd
980
```

##### 4. 在 vsftpd.conf 中手动添加

```bash
local_root=/home/yexiang/ftp_share  // 这里是我自己创建的用于ftp目录
allow_writeable_chroot=YES
```

##### 5. 在 vsftpd.chroot_list 中 添加用户名

```bash
yexiang@ubuntu:<ftp_share>$ cat  /etc/vsftpd.chroot_list    
yexiang
```

##### 6. 举例

- 客户端机器上连接虚拟机上ftp服务器，并上传文件

```bash
yexiang@ubuntu:<~>$ ftp 10.0.4.196
Connected to 10.0.4.196.
220 Welcome to Y/X FTP service.
Name (10.0.4.196:yexiang): yexiang
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> put ccc.txt
local: ccc.txt remote: ccc.txt
200 PORT command successful. Consider using PASV.
150 Ok to send data.
226 Transfer complete.
62 bytes sent in 0.01 secs (4.1174 kB/s)
```

- 虚拟机上ftp服务器查看上传状态

```bash
yexiang@ubuntu:<ftp_share>$ ls
ccc.txt  test_ftp.txt
```



###### 知识点：

> FTP配置文件vsftpd.conf关于限制用户在默认目录的配置，涉及到三个字段：chroot_local_user,chroot_list_enable,chroot_list_file
>
> 我们按顺序配置下来吧！首先，要限制用户在默认目录必须将chroot_local_user设置为yes，即chroot_local_user=yes。此时，用户登录之后，执行目录跳转命令，如cd /home，显示550 Failed to change directory。你可以试试其他的账号，应该都是一样的结果，无法跳转目录
>
> 接下来的问题是，我想对某些用户开个小灶，怎么办呢？那就要说到后面的两个字段啦！
>
> **设置如下：chroot_list_enable=yes**
>
> chroot_list_file=/路径/vsftpd.chroot_list，这里的路径你可以自己指定，之后你要到指定的路径下面创建vsftpd.chroot_list文件
>
> 上面两个设置的意思是（我自己的理解，呵呵！），第一个说明chroot_list这个列表有用，第二个是指明列表的位置。那接着说这个列表的作用，也就是这个vsftpd.chroot_list的作用吧
>
> **做个试验**
>
> - 1.创建两个用户账号，first 和second
> - 2.在上面配置指定的路径下面创建vsftpd.chroot_list文件，将first账号写入文件。
> - 3.用first登录，然后执行目录跳转命令，发现可以成功跳转；用second登录，同样执行目录跳转命令，发现跳转失败。
>
> 接下来我们从另外一方面看，如果注释掉chroot_local_user=yes，再做上面的实验，结果刚好相反，first不能随意跳转，而second可以随意跳转
>
> 结论：写入vsftpd.chroot_list文件的用户账号是有特权的账号，别人都可以不可以跳的时候，它可以随意跳转；别人都可以跳的时候，它不可以跳，呵呵！写入这个文件的账号太有个性啦！



