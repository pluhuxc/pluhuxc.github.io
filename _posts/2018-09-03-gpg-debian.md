---
layout: post
title: debian系Linux的gpg密钥问题解决
tags: [Linux, debian, parrot, ubuntu, kali]
author: luhux
comment: true
---

apt update 提示gpg错误的解决方法

下载新gpg并更新：

[debian](http://mirrors.ustc.edu.cn/debian/pool/main/d/debian-archive-keyring/)

找到并下载最新的密钥安装包

下载。

使用dpkg -i 文件名 安装安装包

解决

如果为ubuntu就去ubuntu源里面下载安装：

[ubuntu](http://mirrors.ustc.edu.cn/ubuntu-ports/pool/main/u/ubuntu-keyring/)

其他发行:

[kali](http://mirrors.ustc.edu.cn/kali/pool/main/k/kali-archive-keyring/)
[parrot](http://mirrors.ustc.edu.cn/parrot/pool/main/p/parrot-archive-keyring/)


如果你想使用其他基于debian的发行的源安装工具你可以添加源后添加密钥使用他们

如果你为arm请不要使用不匹配当前发行的debian发行源进行 apt upgrade 请安装工具后禁用源

参考:

[debian变异成parrotsec](https://parrotsec-cn.org/t/debian-parrot/578)
