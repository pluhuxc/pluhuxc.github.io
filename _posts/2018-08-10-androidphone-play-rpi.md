---
layout: post
title: 使用安卓手机玩raspberrypi
tags: [android, linux, usb, dd, sdcard]
author: Luhux
comment: true
---
# 建议认真读完整篇教程并动脑思考后再进行操作

### 硬件使用:

rapsberrypi

microusb供电线

SD卡

usb数据线

安卓手机(root, 有内存卡插槽或otg和读卡器)


### 软件:

juicessh (android)

解压软件 (android)

带有下载功能的浏览器 (android)

### 操作人要求:

#### 有脑子

会打字

拥有可以理解LINUX的头脑

懂得Linux基础操作

懂得相对路径和绝对路径

会用Linux终端下任意一个及一个以上的编辑器

懂得外置内存卡和内置内存卡的区别

懂得dd 的用处以及误操作的后果

会百度

## 下载raspbian镜像:

[raspbian](https://www.raspberrypi.org/downloads/raspbian/)


下载完成后使用解压软件将下载的压缩包内的文件

解压压缩文件内的img文件

移动到/sdcard/

并更名为raspbian.img

## if 手机有内存卡插槽:

* 手机关机

* 装入内存卡

* 手机开机

* 打开手机设置

* 检查是否挂载内存卡,如有挂载请卸载

* 打开juicessh并新建一个本地终端的连接并执行以下命令

请充分了解dd的功能后再执行:

```
 $ su
 # ls /dev/mmcblk1  #确认目标设备文件是否存在
 # ls /sdcard/raspbian.img  #确认镜像文件是否存在
 # dd bs=4194304 if=/sdcard/raspbian.img of=/dev/block/mmcblk1  #写入镜像到sd卡
```

* bs=4194304 安卓的dd默认并不支持单位(4M x 1024 x 1024 = 4194304)

* /dev/block/mmcblk1 在emmc手机上至少是这样(emmc:mmcblk0 sd:mmcblk1)

等待它写入完毕

* 重启手机

* 使用文件管理器在sd卡根目录创建ssh空文件

* 手机关机

* 取出内存卡

* 手机开机

* 内存卡装入raspberrypi

## if 手机没内存卡插槽或者不想扣电池

* 将sd卡插入读卡器

* 读卡器插入otg

* otg插入手机

* 打开手机设置

* 检查是否挂载读卡器

* 如有挂载请卸载

* 打开juicessh并新建一个本地终端的连接并执行以下命令

请充分了解dd的功能后再执行:

```
 $ su
 # ls /dev/sda  #确认目标设备文件是否存在
 # ls /sdcard/raspbian.img  #确认镜像文件是否存在
 # dd bs=4194304 if=/sdcard/raspbian.img of=/dev/block/sda  #写入镜像到sd卡
```

> bs=4194304 安卓的dd默认并不支持单位(4M x 1024 x 1024 = 4194304)

> /dev/block/sda 在emmc手机上至少是这样(emmc:mmcblk0 otg:sda)

* 等待它写入完毕

* 重启手机

* 使用文件管理器在sd卡根目录创建ssh空文件

* 关闭手机

* 取出内存卡

* 内存卡装入raspberrypi

## 开机

* raspberrypi上电

* 并等待开机

## 连接

* usb连接手机

* 打开手机设置并打开usb共享网络开关

* 打开juicessh,打开本地终端并执行:

```
$ su
# busybox arp -a  # 查看raspberrypi ip

```

一般为192.168.4x.xxx

记住IP

退出本地终端

新建ssh连接

IP: 刚才记下的ip

端口: 22

用户名: pi

密码: raspberry
或: raspberrypi

如果连接失败请检查连接,配置

连接后对raspberrypi进行操作


