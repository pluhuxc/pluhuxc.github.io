---
layout: post
title: 扔掉鼠标
tags: [keynav, i3wm, Debian, Linux]
author: luhux
comment: true
---

# 扔掉鼠标

environment:

- Debian 9 最小化安装版本
- Keyboard

默认无GUI环境。
## 1.安装x服务以及中文字体

	# sudo apt install xserver-xorg xinit ttf-wqy-zenhei -y

- xserver-xorg : X服务基础文件
- xinit : 提供startx命令

## 2.安装窗口管理器i3wm

i3wm官网: [i3wm](https://i3wm.org/)

	# apt install i3-wm dwm -y
	
启动配置:
	
	$ echo 'exec i3' >> ~/.xinitrc  # 将启动命令写入.xinitrc以使用startx启动i3wm

## 3.安装鼠标代替keynav

keynav官网: [keynav](https://www.semicomplete.com/projects/keynav/)

	# apt install keynav -y
	
配置:

	$ cp /usr/share/doc/keynav/keynavrc ~/.keynavrc  # 复制默认配置文件
	
使用文本编辑器打开~/.keynavrc ：
- 查找行 # daemonize 然后去掉注释# 以使keynav以守护进程后台运行
- 查找行 3 click 3 改为 o warp,click 3,end 定义o为鼠标右键点击指定位置

## 4.安装终端xfce4-terminal

	# apt install xfce4-terminal -y
	
## 5.安装中文输入法fictx-googlepinyin

	# apt install fcitx-googlepinyin
	
使用文本编辑器新建~/fcitxenv.sh

写入:

```
#!/bin/sh
export GTK_IM_MODULE=fcitx
export XMODIFIERS=@im=fictx
fcitx &

```

保存

## 6.配置

	$ startx  # 启动图形界面
	
自己选择配置mod按键为windows徽标键

打开终端: mod键+回车

	$ keynav  # 启动keynav
	
keynav基本用法:
- Ctrl + ;  启动keynav
- 启动后可用快捷键:
- h,j,k,l 不同方向分割十字准星
- H,J,K,L 平移十字准星
- 回车 鼠标左键单击十字准星瞄准的地方
- o 右键单击指定位置(非默认快捷键,上方用户配置)

启动fcitx:

	$ source fcitxenv.sh  # 设置变量并启动fcitx
	$ fcitx-configtool # 启动fcitx配置窗口
	
窗口左下角+按钮 > 取消Only show Current Language > 搜索框输入googlepinyin > 选择google拼音 > ok按钮 > 退出配置窗口

## 7.使用

	# reboot  # 重启
	
登录终端

	$ startx  # 启动图形界面
	
WIN键 + enter 打开终端

	$ source fcitxenv.sh  # 启动输入法
	$ keynav  # 启动keynav鼠标

i3使用:
- WIN + enter 启动终端
- WIN + d 启动打开应用菜单
- WIN + shift + q 关闭当前窗口

- WIN + 1~9 切换工作区
- WIN + shift + 1~9 移动当前窗口到某工作区

dmenu使用技巧：可以在选择框中输入程序启动命令前缀来快速定位

更多使用i3wm请参考i3wm官网

配上一张我的使用截图:

![warzone2100](https://raw.githubusercontent.com/luhux/images/master/2018-07-13-194337_1440x900_scrot.png)

(keynav玩的好，游戏难不倒)
