---
layout: post
title: LinuxDeploy 详解
tags: [linux, kali, android,debian ,gentoo]
author: Luhux
comment: true
---

# Linux Deploy 指南

## 本文档排版约定:
界面的文字表示方式:

使用竖屏方式辨认:

```
软件主界面 = 刚打开linuxdeploy显示的界面
左滑动菜单栏 = 左上角的滑动菜单
linux设置区 = 右下角的设置(或下载)标志按钮内
操作菜单 = 主界面右上角的3个点点开
app设置 = 滑动菜单里面的设置
```

## 需要环境

* 拥有root权限

* 拥有300MB+的空余空间

* 已安装busybox

## 安装linuxdeploy

下载地址:

[github](https://github.com/meefik/linuxdeploy/releases)

本教程使用Linux Deploy 2.2.0

## 初始化运行环境

打开Linuxdeploy


软件主界面 -> 左滑菜单栏 -> app设置 -> 勾选: 启用cli -> 点更新环境 -> 授权root -> 等待更新完成


## 设置linux

软件主界面 -> linux设置区

### 引导设置

#### 容器类型

建议使用 chroot 

* proot 目前在linuxdeploy不完备,可能无法在安装

#### 发行版GNU/linux

选择你需要的发行

#### 架构

选择你手机支持的架构

#### 发行版GNU/Linux版本

选择你需要的发行的版本

#### 源地址

建议使用国内的源地址

#### 安装类型

##### 镜像文件

将GNU/linux安装到一个镜像文件(虚拟磁盘)中

如果你将GNU/linux安装到fat32的sd卡中请使用该选项

安装路径为镜像文件的绝对路径 默认为/sdcard/linux.img

镜像大小为镜像文件的大小 默认为自动

> 如果镜像路径设置在fat32内存卡的目录下镜像大小应不大于4096mb (fat32文件系统文件大小限制)

文件系统为镜像文件内要使用的分区的文件系统 默认为ext4

##### 目录

将GNU/linux安装在一个ext4或f2fs分区的一个目录里

安装路径为要安装到的目录 默认为linuxdeploy的应用数据目录(在data分区)

> 安装目录请勿使用fat32分区的目录,以及/sdcard/ 内部共享存储目录, 它们不支持linux文件系统的基本权限

##### 分区

格式化一个分区并将GNU/Linux安装到里面

安装路径为一个块设备分区文件的绝对路径 默认为/dev/block/mmcblkXpY

> 如果要安装到内存卡的第一个分区请写: /dev/block/mmcblk1p1

> 如果要安装到otg u盘的第一个分区请写: /dev/block/sda1

文件系统格式化分区所指定的文件系统 默认为ext4

##### RAM

创建一个ramdisk并将linux安装到里面

安装路径为ramdisk的挂载路径 默认为/data/local/ram

镜像大小为ramdisk的大小

> ramdisk会在关机后清除

#### 用户名

要创建的普通账户的用户名

#### 用户密码

要创建的普通账户的密码

> 建议别设置过于简单或简短的密码,否则可能设置失败

#### 特权用户

默认为root

建议不要修改

#### DNS

指定GNU/Linux要使用的DNS地址

默认为自动

#### 本地化

设置GNU/Linux要使用的本地化设置(语言设置)

默认为POSIX 标准英语
中文请设置为
```
zh_CN.UTF-8
```

### 初始化

初始化为linuxdeploy启动linux时自动执行的脚本设置

#### 初始化系统

##### run-parts

在启动linux时执行指定的脚本

初始化路径 要执行的脚本的路径
初始用户 要执行脚本的用户

##### sysv

如果你安装的linux支持并安装sysv请选择此选项

初始化级别为init运行类型 默认为3(default)

初始用户为运行init的用户 默认为root

### 挂载

将GNU/linux外部的一个目录挂载到GNU/linux供linux访问

> 请勿在挂载点列表填写块设备文件名

### SSH

启用SSH连接方式

端口ssh服务启动的端口 默认22

### PulseAudio
启用PulseAudio服务并转发linux内的音频输出到目标pulseaudio服务器

安卓可使用Pulsedroid作为输出服务器

也可以转发到其他拥有pulseaudio的设备上

host 目标主机ip

port 目标主机运行pulseaudio服务的端口

### 图形界面

#### 图形子系统

桌面环境为要使用启动的桌面启动环境

使用安装的Linux里面的用户Home目录下的文件控制

```
x11: ~/.xinitrc
vnc: ~/.vnc/xstartup
framebuffer: ~/.xinitrc
```

> 如果图形连接不显示或者启动错误请修改x服务启动控制文件

##### vnc

启用vnc作为图形连接方式

vnc设置:

显示为要使用的显示DISPLAY变量 影响端口 默认为0 

建议不要修改

与vnc监听端口的关系:
```
显示 0 端口 5900
显示 1 端口 5901
```
颜色深度为显示的颜色深度 默认为16bit 影响vnc画质

8bit 低画质 低网络占用

dpi为显示密度 默认为 75 dpi越高显示密度更大

建议保持默认不要修改


宽 高 为vnc的显示分辨率 默认为手机横屏取值 

vnc选项 为vnc扩展选项默认空

##### x11

启用x11服务来访问图形

Linuxdeploy并不提供x11的服务器

x11服务的连接方式为:

```
x11客户端 (linuxdeploy里面的linux)
          |^
          ||
          v|
      x11服务器
```

如果要在安卓自身使用x11服务请安装安卓的XsdlServer app

并按照xsdlserver显示的设置x11服务

##### framebuffer

停止/暂停/冻结 安卓界面并让linux使用fb设备显示图形

> 在大多数设备上需要设置停止安卓界面来显示linux图形界面

> 在许多设备上无法此显示方式工作

显示为要使用的DISPLAY变量 默认为0 建议不要修改

视频设备为要使用的fb设备文件 建议不要修改

输入设备为触摸屏设备event设备文件 用于启用图形触摸屏支持 建议不要修改

x参数为x指定附加参数 建议不要修改

强制刷新缓存区 建议勾选

冻结安卓界面 建议选择停止

## 开始安装

主界面 -> 操作菜单 -> 安装 

开始安装并等待安装

安装结束后会在最后几行输出
```
<<<deploy
```

## 验证安装

主界面 -> 启动 

打开手机上的终端软件

执行:

```
android $ su
root # linuxdeploy shell -u root
```

如果顺利进入linux的shell说明安装成功

如果无法顺利进入请停止并检查安装,或者停止重新安装

## 连接ssh服务

如果手机自身连接请下载安卓ssh客户端:

* connectbot
* juicessh

并连接localhost和你设置的ssh端口 (端口默认为22)
使用你设置的用户名密码登录

可选操作:

> 登录后请更改默认密码为强密码

> 如果外部连接本linux请输入手机的局域网ip地址并连接指定ssh端口

## 连接vnc服务

手机自身连接

下载安卓vnc客户端:

* vncviewer

连接localhost并输入密码为用户设置密码(用户名下面的那个)

> vnc密码和账户密码没有关联,只是linuxdeploy设置的密码在安装时候会共享

## 连接x11

手机自身连接

下载xsdlserver并启动

启动xsdlserver后启动linuxdeploy里面的linux

## 使用framebuffer

启动linux的同时手机会黑屏

如果framebuffer启动成功则显示linux的图形界面

如果失败则黑屏 请手动重启

在linuxframebuffer启动的时候可以使用外部ssh到手机来操作



# FAQ

问: linuxdeploy安装的linux可以运行什么?
答: linuxdeploy安装的linux是一个完备的linux系统,你可以使用包管理器或者编译方式扩展它的功能. 唯一限制为使用安卓的内核运行(你可能无法使用一些只提供安卓接口的硬件)

问: linuxdeploy安装的linux里面安装了xrdp,但为什么不能运行?
答: 安卓拥有一套特别的权限管理方式 ,由于xrdp在单独一个叫xrdp的账户运行守护进程, 所以需要把xrdp用户加入*aid_inet*用户组 : 
```
root@linux # usermod -aG aid_inet xrdp  # 将xrdp加入套接字权限组 ,其他需要套接字权限的应用也可以这样设定 
```

问: 我想了解linuxdeploy的工作原理.
答: 去github:[linuxdeploy](https://github.com/meefik/linuxdeploy),可以的话给开发者点个star


