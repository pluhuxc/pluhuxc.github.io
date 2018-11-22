---
layout: post
title: 在slackware上使用alsa管理音频
tags: [alsa, linux, audio ,slackware]
author: Luhux
comment: true
---


在slackware默认使用pulse管理音频，在没有安装pulse情况下 alsamixer 会有一堆报错使音频无法使用

解决:

    # rm -rf /etc/asound.conf
	
删除slackware自带的asound.conf

