---
layout: post
title: 搭建一个proot环境来交叉编译
tags: [arm, linux, ubuntu, qemu, proot]
author: Luhux
comment: true
---

>你可以使用这个办法测试你的代码是否可以在arm平台运行编译

>它并不模拟全部硬件

>这是一个很懒的办法

>这个办法编译有点慢,使用qemu模拟

>驱动建议别使用此方法编译

使用: 

* proot 
* qemu-user-static 
* ubunturootfs

proot,qemu-user-static自行在包管理器中安装

Ubunturootfs:

[ubunturootfs](http://mirrors.ustc.edu.cn/ubuntu-cdimage/ubuntu-base/releases/)

自行选择架构版本

我要为香橙派lite (armv7)编译ons所以选择了armhf的rootfs

下载,解压至rootfs文件夹

配置网络:

	rootfs $ cp /etc/hosts ./etc/
	rootfs $ cp /etc/resolv.conf ./etc/
	
使用proot和qemu切换进rootfs:

	rootfs $ proot -r ./ -q /usr/bin/qemu-static-arm  #-q参数根据rootfs架构选择
	# bash 
	root@localhost # 
	
apt update:

	root@localhost # apt update

自行安装所需工具链

编译完成后把编译完成的二进制库文件等拷贝到板子上就可以使用

提示却少共享库请自行补全,或者拷贝rootfs里面的
