---
layout: post
title: 活在Emacs中exwm
tags: [emacs, linux, keynav]
author: Luhux
comment: true
---

* 环境: LINUX 
* 软件: emacs


### exwmwiki: 

[exwm](https://github.com/ch11ng/exwm/wiki)

## 安装exwm

M-x RET package-install RET exwm RET

## 启用exwm

在emacsinit文件中添加:

```
;; Emacs is my windowsmanager 
(require 'exwm)
(require 'exwm-config)
(exwm-config-default)
```

设置默认启动emacs作为窗口管理器:

	$ cp ~/.emacs.d/elpa/exwm*/xinitrc ~/.xinitrc  # 复制exwm启动配置文件为默认使用xinit启动配置文件

## 配置

### 为emacs启用中文输入法(可选)

#### emacs内置输入法

> 拼音输入用户推荐使用pyim

在 ~/.emacs 文件中加入:

```
;; buildin chineseIME
(setq default-input-method "chinese-cns-tsangchi")  # 仓颉输入法chinese-py为拼音输入法
(global-set-key (kbd "C-\\") 'toggle-input-method)  # 设置C-\切换输入法
```
>内置pinyin输入法词库很差,可以使用pyim代替

>此输入法只能在emacs编辑区使用

#### 配置fcitx输入法

在~/.xinitrc中加入

> 加在启动Emacs的命令之前

```
#fcitx IME

export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERs=@im=fcitx
fcitx-autostart &
```


## 启动exwm:

建议切换启动级别为init 3

tty登录普通用户

执行
```
$ startx
```

启动图形界面并启动exwm

## 使用:

s键为super键

启动应用:

使用s-&并输入启动的x应用的命令

##### 例如:

启动xfce4-terminal

s-&

```
$ xfce4-termianl
```

>可以使用Emacs的缓存区来管理启动的应用

### 使用工作区

s-N

N为数字键,切换到指定工作区,如果工作区不存在,将会创建新的工作区

s-w

显示工作区列表并选择

C-c C-m 

将正在使用的应用移动到其他工作区

### 快捷键冲突的解决

默认应用启动于line-mode所以可以使用Emacs的快捷键对应用操作

遇到快捷键冲突可使用fullscreen mode来解决

C-c C-f

进入fullscreenmode

此时Emacs快截键在这个模式被禁用

返回line-mode:

s-r

## 问题解决

### Firefox 显示不完全解决:

启动firefox之后两次F11

### 切换 fcitx 输入法按键

切换到无Emacs键位绑定位置

比如:

Ctrl + '


![emacs-exwm](https://raw.githubusercontent.com/luhux/images/master/Emacs-exwm.png)
