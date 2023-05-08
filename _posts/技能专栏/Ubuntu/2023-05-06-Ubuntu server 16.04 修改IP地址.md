---
layout: post
title: Ubuntu server 16.04 修改IP地址
date: '2023-05-06'
description: 'Ubuntu server 16.04 修改IP地址'
categories: ['【👉👉👉 技能专栏】','【Ubuntu】']
tags: [技能,Ubuntu]
---


#### 1. 修改 Sudo vim /etc/network/interfaces

```elm
#iface ens33 inet dhcp
iface ens33 inet static
address  192.168.1.117
netmask 255.255.255.0
gateway 192.168.1.253    
```

- 保存退出 : wq



#### 2. 重启网络

```elm
sudo /etc/init.d/networking restart     
```



#### 3. 修改/etc/NetworkManager/NetworkManager.conf

- 将 managed=false 修改为 managed=true，实例如下：

```elm
[main]

plugins=ifupdown,keyfile

[ifupdown]

#managed=false

managed=true
```

-  检查服务状态：
-  使用命令 `netstat -an | more` 命令查看将出现如下信息：

```elm
udp 0 0 10.106.57.71:69 0.0.0.0:*
```