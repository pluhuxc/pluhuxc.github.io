---
layout: post
title: 编译安装lua
tags: [lua, linux]
author: Luhux
comment: true
---



下载:

    build $ wget https://www.lua.org/ftp/lua-5.1.5.tar.gz
	
解压:

	build $ tar xf lua-5.1.5.tar.gz
	
make:

	lua-5.1.5 $ make linux
	
安装:

	lua-5.1.5 $ sudo make install
