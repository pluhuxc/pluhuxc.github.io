---
layout: post
title: fbterm 中中文乱码的解决
tags: [fbterm, linux, terminal, utf8, cn]
author: Luhux
comment: true
---


fbterm 乱码的原因:

# 编码问题


在我刚安装的slackware上默认编码不是utf8 而是ISO-xxxx

解决:

    $ echo 'export LANG="en_US.utf8"' >> ~/.bashrc
	
重新进入终端解决
