---
layout: post
title: keynav
tags: [keynav, Debian, Linux]
author: luhux
comment: true
---

# keynav 用法

## 1.安装
Debian:

	# apt-get install keynav
	
## 2.使用
默认

启动:

    $ keynav &  # 启动keynav并挂在后台运行

Ctrl+;   进入keynav分割模式

如图:
![keynavfullscreen](https://raw.githubusercontent.com/luhux/images/master/2018-07-13-194337_1440x900_keynavfullscreen.png)

分割模式内常用快捷键:

快捷键 | 功能
-------|------
h,j,k,l | 向左,下,上,右分割红框
H,J,K,L | 向左,下,上,右移动红框
y,u,b,n | 左上,右上,左下,右下分割红框
Y,U,B,N | 左上,右上,左下,右下移动红框
Esc | 退出分割模式
Space | 左键单击击红框中心位置
; | 将鼠标光标移动到红框中心位置
w | 将红框移动缩放到活动的窗口
c | 将红框移动缩放到鼠标所在位置并缩放为指定大小(默认200x200)
a | 撤销操作

默认操作中没有鼠标右键右键单击
所以手动添加:

    cp /usr/share/doc/keynav/keynavrc ~/.keynavrc  # 复制默认配置文件到home
	
使用文本编辑器打开并加入:

	o warp,click 3,end  # 绑定o键为: 鼠标右键单击红框中心位置
	
更多自定义请man keynav 并自定义~/.keynavrc配置文件
