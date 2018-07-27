---
layout: post
title: About this Jekyll theme
tags: [Emacs, irc, qq, slackware, arm]
author: luhux
comment: true
---

arm无法运行wineqq

slackwarearm又装不了中文输入法去上smartqq

中文只是为了用emacs写博客和代码注释

所以用emacs&pyim输入法&mojoqq来实现上qq的功能

mojoqq：

[Mojoqq](https://github.com/sjdy521/Mojo-Webqq)


安装cpanm：

    # cpan -i App::cpanminus

安装mojoqq：

	# cpanm Mojo::Webqq
	
安装mojoqqirc模块：

	# cpanm -v Mojo::IRC::Server::Chinese
	
创建启动脚本：

    $ echo '#!/usr/bin/env perl
    use Mojo::Webqq;
    my $client = Mojo::Webqq->new();
	$client->load("ShowMsg");
	$client->load("IRCShell"); #加载IRCShell插件
	$client->run();' > ircqq.pl
	
emacs:

### 使用melpa源进行安装

安装pyim中文输入法:

    M-x package-install
    pyim

配置：
#### 我只使用最小化拼音输入
打开emacsinit文件
写入：

	;;pyim
    (require 'pyim)
    (require 'pyim-basedict)
    (setq default-input-method "pyim")
    (global-set-key (kbd "C-\\") 'toggle-input-method)
	
保存

使用ctrl＋\切换输入法状态

使用数字选择候选字

Ctrl + f,b,n,p 进行候选字翻页

enter输入原字符


## emacs&mojo 使用：

	$ perl ircqq.pl
	
等待登录二维码下载完毕

读取二维码：

	$ xv /tmp/mojo_webqq_qrcode_default.png

拿手机扫描二维码

打开emacs

打开erc

    M-x erc
	localhost
	6667
	yournickname
	
有新消息被接收时会在emacs里面新开一个缓存区

[emacsqq](https://raw.githubusercontent.com/luhux/images/master/Emacsqq.png)

## 转载请注明

## 作者:luhux

## 邮箱: luhux@hotmail.com
