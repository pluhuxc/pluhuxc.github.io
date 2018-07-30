---
layout: post
title: 在slackwarearm上编译安装gucharmap
tags: [arm, linux, slackware, src]
author: luhux
comment: true
---

下载gucharmap:

	build $ wget ftp.gnome.org/pub/GNOME/sources/gucharmap/10.0/gucharmap-10.0.4.tar.xz
	
解压：

	build $ tar xf gucharmap-10.0.1.tar.xz

configure:

	gucharmap-10.0.4 $ ./configure
	
报错:

	checking for Unicode data... configure: error: You need to specify the path to the unicode data files with --with-unicode-data=PATH, or use --with-unicode-data=download to download the data files during the build process.
	
尝试:

	gucharmap-10.0.4 $ ./configure --with-unicode-data=download
	
编译:

	gucharmap-10.0.4 $ make 
	
报错:

	/usr/bin/ld: gucharmap-main.o: undefined reference to symbol 'dlsym@@GLIBC_2.4'
	/lib/libdl.so.2: error adding symbols: DSO missing from command line
	
尝试:

	gucharmap-10.0.4 $ make clean
	gucharmap-10.0.4 $ ./configure --with-unicode-data=download LIBS='-ldl'
	gucharmap-10.0.4 $ make
	
成功

安装:


	gucharmap-10.0.4 $ sudo make install
	

最后：
发现slackware包管理器里有这个包。
(苦死)
