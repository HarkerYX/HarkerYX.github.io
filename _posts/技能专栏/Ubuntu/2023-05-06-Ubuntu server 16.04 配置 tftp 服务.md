---
layout: post
title: Ubuntu server 16.04 配置 tftp 服务
date: '2023-05-06'
description: 'Ubuntu server 16.04 配置 tftp 服务'
categories: ['【👉👉👉 技能专栏】','【Ubuntu】']
tags: [技能,Ubuntu]
---

##### 安装配置：

###### 1. 安装tftp-hpa，tftpd-hpa , tftp-hpa是client，tftpd-hpa是server

```bash
sudo apt-get install tftp-hpa tftpd-hpa
```

###### 2. 配置tftpd-hpa

```bash
sudo vim /etc/default/tftpd-hpa 
 
TFTP_USERNAME="tftp"
TFTP_DIRECTORY="/tftp_share" 这里是你的tftpd-hpa的服务目录,这个想建立在哪里都行
TFTP_ADDRESS="0.0.0.0:69"
TFTP_OPTIONS=" -l -c -s"  这里是选项,-c是可以上传文件的参数，-s是指定tftpd-hpa服务目录，上面已经指定
```

###### 3. 目前权限

```bash
chmod 777 /tftp_share
```

######  4. 启动服务

```bash
sudo service tftpd-hpa restart # 启动服务，这里要注意，采用的独立服务形式。
```



##### 测试：

```bash
# cd /home
 
# tftp localhost  #localhost 表示本机
tftp>get test.txt  //test.txt 是之前在 /tftpboot 目录下新建的文件
tftp>put test1.txt //test1.txt 是在 /home 目录下新建的文件
tftp>q
 
#退出后，在/home目录下会有一个test.txt文件，在/tftpboot 目录下有test1.txt，表示tftp服务器安装成功！
```
