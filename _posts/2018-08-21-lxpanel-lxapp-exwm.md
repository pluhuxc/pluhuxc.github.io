---
layout: post
title: 为EXWM添加lxpanel
tags: [lxde, exwm, linux, emacs]
author: Luhux
comment: true
---

只有emacs的窗口管理方式效轳很低

## 安装lxpanel

	$ sudo apt install lxpanel -y

## 添加自启

在~/.xinitrc中的启动emacs命令前添加:

```
# lxpanel
lxpanel &
```
![exwm-and-lxpanel](https://raw.githubusercontent.com/luhux/images/master/exwm-and-lxpanel.png)
