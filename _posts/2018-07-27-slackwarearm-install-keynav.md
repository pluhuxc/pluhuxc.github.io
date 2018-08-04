---
layout: post
title: 在slackware安装keynav
tags: [key, slackware, linux, arm]
author: luhux
comment: true
---

安装了slackwarearm
发现包管理器里没有keynav

在github找到了一个deb转txz的工具:

[deb2tgz](https://github.com/vborrego/deb2tgz.git)

    # git clone https://github.com/vborrego/deb2tgz.git
	# cp deb2tgz/deb2tgz /usr/local/bin
	
安装好后在debian源中找到了keynav的armhf包

[keynav](http://ftp.debian.org/debian/pool/main/k/keynav/)

    # wget http://ftp.debian.org/debian/pool/main/k/keynav/keynav_0.20110708.0-1_armhf.deb
	# deb2tgz keynav_0.20110708.0-1_armhf.deb
	# installpkg keynav_0.20110708.0-1_armhf.txz
	
    $ keynav
	keynav: error while loading shared libraries: libxdo.so.3: cannot open shared object file: No such file or directory

keynav依赖于libxdo库

[libxdot](http://ftp.debian.org/debian/pool/main/x/xdotool/)

	# wget http://ftp.debian.org/debian/pool/main/x/xdotool/libxdo2_2.20100701.2961-3+deb7u3_armhf.deb
	# deb2tgz libxdo2_2.20100701.2961-3+deb7u3_armhf.deb
	# installpkg libxdo2_2.20100701.2961-3+deb7u3_armhf.txz
	
	$ keynav
	keynav: error while loading shared libraries: libxdo.so.3: cannot open shared object file: No such file or directory
	
	# find /usr/lib | grep libxdo.so.3
	/usr/lib/arm-linux-gnueabihf/libxdo.so.3
	# ln -s /usr/lib/arm-linux-gnueabihf/libxdo.so.3 /usr/lib/libxdo.so.3
	
	$ keynav
	
使用快捷键Ctrl + ;出现红十字框，安装成功

slackware安装deb包问题解决思路:
1. 解决依赖
2. 检查路径


## 转载请注明

## 作者:luhux

## 邮箱: luhux@hotmail.com
