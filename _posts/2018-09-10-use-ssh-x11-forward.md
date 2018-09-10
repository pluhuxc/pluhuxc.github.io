---
layout: post
title: 使用ssh x11转发连接linux图形
tags: [linux, ssh, x11, network, display]
author: Luhux
comment: true
---

ssh服务器： Ubuntu bionic armv7l 无图形

> shell标识: user@Ubuntu 

ssh客户端:  Debian strech armv7l 已经开启图形

> shell标识: user@Debian

## 连接到ssh服务器上：

    Debian@debian $ ssh ubuntu@192.168.1.100

## 修改ssh配置文件:

打开配置文件

    root@Ubuntu # vim /etc/ssh/sshd_config

找到下面内容所在行并修改为:

```
AllowTcpForwarding yes
X11Forwarding yes
X11DisplayOffset 10
X11UseLocalhost no
```

## 重启ssh服务

    root@Ubuntu # service ssh restart

## 安装xauth

    root@ubuntu # apt install xauth

#### 如果x程序中有中文请安装中文字体

    root@Ubuntu # apt install ttf-wqy-zenhei


## 客户端配置

    root@Debian # vim /etc/ssh/ssh_config

找到下列行并修改为默认为下列行:
```
   ForwardX11 yes
   ForwardX11Trusted yes
```

## 初始化xauth

    debian@Debian $ ssh -X ubuntu@192.168.1.100

登录进去后执行exit退出

    debian@Debian $ ssh -X ubuntu@192.168.1.100

登录进去后执行exit退出

> 执行两次是为了创建默认的xauth需要的配置文件

## 以后的使用

    debian@Debian $ xhost +  # 允许任意访问x服务的连接
    debian@Debian $ ssh -X ubuntu@192.168.100

登录后:

    ubuntu@Ubuntu $ firefox   #通过ssh转发到debian机的x服务开启firefox浏览器


> x服务非常耗费网络，至少我上下行10m/s 没有很流畅使用firefox浏览网页

> ssh断开 ，x服务也会跟着断开

![ssh-x11](https://raw.githubusercontent.com/luhux/images/master/ssh-x11-chromium.png)
