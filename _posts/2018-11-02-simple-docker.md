---
layout: post
title: 使用docker
tags: [docker, linux, arm, x86, ustc]
author: Luhux
comment: true
---

# 安装

使用官方静态编译的二进制包:


地址: https://download.docker.com/linux/static/stable/


进去后选择合适架构,下载



	$ wget https://download.docker.com/linux/static/stable/x86_64/docker-18.09.0.tgz
	
	
解压:


	$ tar xf docker-*.tgz 
	

复制到/usr/local/bin/

    # cp docker/* /usr/local/bin/

# 配置


启动守护进程

	# dockerd &
	
测试hello-world

	# docker run hello-world
	
添加docker用户组

	# groupadd docker 
	
把用户加入docker用户组

	# usermod -aG docker USERNAME
	
	



