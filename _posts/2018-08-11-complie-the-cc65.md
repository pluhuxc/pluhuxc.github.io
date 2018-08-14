---
layout: post
title: 编译CC65
tags: [linux, nes, cc65, c]
author: luhux
comment: true
---

[cc65](https://github.com/cc65/cc65/releases)

解压并进入:

make:

	cc65-2.17 $ make
	
make install:

	cc65-2.17 $ mkdir ~/.local
	cc65-2.17 $ make install PREFIX=~/.local
	
把.local/bin 添加到PATH:
编辑.bashrc
加入:
```
export USER_BIN=~/.local/bin
export PATH=$PATH:$USER_BIN:
```

```
$ source ~/.bashrc
$ cc65
no input file
```

安装成功
