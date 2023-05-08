---
layout: post
title: Ubuntu server 16.04 修复丢失的 vmdk
date: '2023-05-06'
description: 'Ubuntu server 16.04 修复丢失的 vmdk'
categories: ['【👉👉👉 技能专栏】','【Ubuntu】']
tags: [技能,Ubuntu]
---

##### 问题：

- 打开虚拟机发现提示 “找不到文件: Ubuntu_server_16.04_100G-cl2.vmdk” 发现确实没有了这个文件，莫名其妙消失

##### 解决方法：（博主已经通过此方法解决）

1. 我是有多个虚拟机 所以就拷贝了一份另外一台虚拟机的 xxx.vmdk 改成了 Ubuntu_server_16.04_100G-cl2.vmdk （如果没有现成的虚拟机，那就快速的安装一个吧，玩虚拟机就应该有多个~）

2. 再次开启确实可以运行起来进入shell，但是发现命令行比如 cd / 按tab按键报错 “mount: cannot remount /dev/sda1 read-write, is write-protected” 原因是 / 目录被mount 为 ro

3. 尝试 sudo mount -o remount,rw /dev/sda1 （这里具体看你那边的磁盘信息），发现也不行

4. 重启

5. 重启后发现虚拟机进入到  （initramfs） 这个命令行下面，可以输入一些命令，但是毕竟不是最终想要的

6. 在（initramfs） 这个命令行下面执行 fsck /dev/sda1 ，会一直提示你输入，一路y就行，然后最后提示修复完成

7. 命令行输入 reboot 重启，发现成功进入，原来的虚拟机以及配置的东西都还在，算是成功修复了吧

