---
layout: post
title: Ubuntu server 16.04 配置 samba 服务
date: '2023-05-06'
description: 'Ubuntu server 16.04 配置 samba 服务'
categories: ['【👉👉👉 技能专栏】','【Ubuntu】']
tags: [技能,Ubuntu]
---

##### 1. 安装 samba

```bash
sudo apt-get install samba samba-client samba-common
```

##### 2. 启动 samba

```bash
# sudo /etc/init.d/samba start 
 
# sudo /etc/init.d/smbd restart
或
# sudo service smbd restart
```

##### 3. 设置 samba 的密码， 可以先添加 samba 用户， 必须是系统用户

```bash
sudo smbpasswd -a yexiang
```

##### 4. 配置 smb.conf

```bash
在 /etc/samba/smb.conf 最后添加 ，其他地方不要改
 
##Y.X ADD share file with windows system
 
[smb_share]
comment = Y.X share file with windows system
path = /home/yexiang/smb_share
public = yes
available = yes
valid users = yexiang
browseable = yes
writable = yes
guest ok = yes
 
#create mask = 0777
#directory mask = 0775
#force user = nobody
#force group = nobody
```

##### 5. samba安装好后，使用 testparm 命令可以测试smb.conf配置是否正确

- 使用 `testparm –v ` 命令可以详细的列出 smb.conf 支持的配置参数

```bash
yexiang@ubuntu:<~>$ testparm -v
Load smb config files from /etc/samba/smb.conf
rlimit_max: increasing rlimit_max (1024) to minimum Windows limit (16384)
WARNING: The "syslog" option is deprecated
Processing section "[printers]"
Processing section "[print$]"
Processing section "[smb_share]"
Loaded services file OK.
Server role: ROLE_STANDALONE
```

- **如果testparm -v 出现错误**

```bash
"rlimit_max: increasing rlimit_max (1024) to minimum Windows limit (16384)"
```

- **需要修改 /etc/security/limits.conf**

```bash
#*               soft    core            0
#root            hard    core            100000
#*               hard    rss             10000
#@student        hard    nproc           20
#@faculty        soft    nproc           20
#@faculty        hard    nproc           50
#ftp             hard    nproc           0
#ftp             -       chroot          /ftp
#student        -       maxlogins        4
*                -       nofile          16384 ## 添加这行
```

- **重启系统生效**



##### 6. \\\\samba服务IP地址 可以看到共享的目录

