---
layout: post
title: 编译安装aircrack-ng
tags: [aircrack, linux, arm, slackware]
author: luhux
comment: true
---

[aircrack](http://www.aircrack-ng.org/doku.php?id=downloads#linux_packages)

下载:

	build $ wget http://download.aircrack-ng.org/aircrack-ng-1.3.tar.gz
	
墙了,换:

	build $ git clone https://github.com/aircrack-ng/aircrack-ng
	
autogen.sh:

	aircrack-ng $ ./autogen.sh
	
make:

	aircrack-ng $ make
	
install:

	aircrack-ng $ make install
