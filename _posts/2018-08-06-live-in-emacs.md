---
layout: post
title: 活在Emacs中exwm
tags: [emacs, linux, keynav]
author: Luhux
comment: true
---

* 环境: LINUX 
* 软件: emacs

为Emacs加入MELPA源:

编辑 ~/.emacs 文件:

加入:

```
;; melpa
(require 'package)
(add-to-list 'package-archives '("melpa" . "https://melpa.org/packages/"))
(add-to-list 'package-archives '("melpa-stable" . "https://stable.melpa.org/packages/"))
(package-initialize)
```
exwmwiki: 

[exwm](https://github.com/ch11ng/exwm/wiki)

安装Emacs 窗口管理器 exwm:

M-x RET package-install RET exwm

启用exwm:
```
;; Emacs is my windowsmanager 
(require 'exwm)
(require 'exwm-config)
(exwm-config-default)
```

设置默认启动emacs作为窗口管理器:

	$ cp ~/.emacs.d/elpa/exwm*/xinitrc ~/.xinitrc  # 复制Exwm启动配置文件为默认使用x启动配置文件


启用中文输入法(可选):

>拼音输入用户推荐使用pyim
>你也可以在~/.xinitrc中加入你的系统输入法启动变量和命令

在 ~/.emacs 文件中加入:

```
;; Use Emacs build in chineseIME
(setq default-input-method "chinese-cns-tsangchi")  # 仓颉输入法 切换chinese-py为拼音输入法
(global-set-key (kbd "C-\\") 'toggle-input-method)  # 设置C-\切换输入法
```
启动exwm:

建议切换启动级别为init3

login登录tty

```
$ startx
```
Emacs没有报错说明启动成功

使用eshell来启动应用:

M-x RET eshell RET

启动firefox
```
$ firefox &
```
关闭firefox
关闭firefox的缓存区:

C-x k firefox RET

启动xfce4-terminal:

C-x b eshell RET

```
$xfce4-termianl &
```

>可以使用Emacs的缓存区管理方式来管理启动的应用
>可以分割窗口(像平铺wm那样使用)

### Firefox 显示不完全解决:

启动firefox之后两次F11

>如果不想使用鼠标,可以使用keynav辅助操作
>内置pinyin输入法词库很差,可以使用pyim代替

![emacs-exwm](https://raw.githubusercontent.com/luhux/images/master/Emacs-exwm.png)
