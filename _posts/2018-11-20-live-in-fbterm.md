---
layout: post
title: 活在fbterm
tags: [linux, fbterm ,framebuffer]
author: Luhux
comment: true
---

在一台512M内存的机子使用xorg显示实在太费
所以用上了fbterm


# 安装

    # apt install fbterm
	
# 修改配置

    $ vim ~/.fbtermrc
	
要修改的几行:
```
# 字体大小
font-size=20

# 前景和后景颜色
color-foreground=3
color-background=0
```

# 启动fbterm

    $ fbterm  # 在tty里面启动
	
# 上网,中文输入,写代码


使用emacs和w3m解决

    # apt install w3m
    
	M-x package-install RET w3m
	

启动emacs

    $ emacs
	
启动w3m

    M-x w3m RET
	
中文输入使用emacs的输入法

在~/.emacs加入

```
;; buildin chineseIME
(setq default-input-method "chinese-tonepy")
(global-set-key (kbd "C-\\") 'toggle-input-method)
```

这样在emacs中使用 Ctrl + \ 就可以输入中文了

* emacs 包管理器有一个 pyim 中文输入法


写代码用emacs.


# 看图片,  看视屏.

安装fbv

    # apt install fbv
	
如果源里面没有的话去下载源码编译

看图:

    $ fbv 图片文件
	

看视屏用mplayer

    # apt install mplayer
	
	
	$ mplayer -vo fbdev 视屏文件
	

# 看pdf

看一些pdf文档

pdf 转图片 

转成一页一页的

	# apt install imagemagick
	
	$ convert pdf文件 要转换的png文件前缀

	$ fbv 文件
	

