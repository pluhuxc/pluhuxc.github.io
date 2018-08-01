---
layout: post
title: 配置一个mini桌面环境
tags: [linux, debian, fluxbox]
author: luhux
comment: true
---
# on debian

安装x :

	# apt install xinit xserver-xorg
	
安装中文字体 :

	# apt install ttf-wqy-zenhei
	
安装fluxbox :

	# apt install fluxbox
	
编辑启动配置文件:

屏幕显示:

编辑~/.xinitrc

加入

    exec fluxbox
	
	
vnc显示:

编辑~/.vnc/xstartup

加入

    fluxbox &
	
	
