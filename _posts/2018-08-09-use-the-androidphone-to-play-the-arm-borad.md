---
layout: post
title: 使用安卓手机玩orangepi
tags: [android, linux, usb, dd, sdcard]
author: Luhux
comment: true
---
# 建议认真读完整篇教程并动脑思考后再进行操作

### 硬件使用:

香橙派

DC供电线

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

懂得apt remove

懂得dd 的用处以及误操作的后果

会百度

## 下载Armbian镜像:

[armbian](http://dl.armbian.com/)


下载完成后使用解压软件将下载的压缩包内的文件

解压压缩文件内的img文件

移动到/sdcard/

并更名为Armbian.img

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
 # ls /sdcard/Armbian.img  #确认镜像文件是否存在
 # dd bs=4194304 if=/sdcard/Armbian.img of=/dev/block/mmcblk1  #写入镜像到sd卡
```

* bs=4194304 安卓的dd默认并不支持单位(4M * 1024 * 1024 = 4194304)

* /dev/block/mmcblk1 在emmc手机上至少是这样(emmc:mmcblk0 sd:mmcblk1)

等待它写入完毕

* 手机关机

* 取出内存卡

* 手机开机

* 内存卡装入orangepi

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
 # ls /sdcard/Armbian.img  #确认镜像文件是否存在
 # dd bs=4194304 if=/sdcard/Armbian.img of=/dev/block/sda  #写入镜像到sd卡
```

> bs=4194304 安卓的dd默认并不支持单位(4M * 1024 * 1024 = 4194304)

> /dev/block/sda 在emmc手机上至少是这样(emmc:mmcblk0 otg:sda)

* 等待它写入完毕

* 取出内存卡

* 内存卡装入orangepi

## 开机

* orangepi上电

* 并等待开机

## 连接

* usb连接手机

* 打开手机设置并打开usb共享网络开关

* 打开juicessh,打开本地终端并执行:

```
$ su
# arp -a  # 查看orangepi ip

```

一般为192.168.4x.xxx

记住IP

退出本地终端

新建ssh连接

IP: 刚才记下的ip

端口: 22

用户名: root

密码: 1234

如果连接失败请检查连接,配置

连接后对orangepi进行操作

初始化完成后

请根据自行设置账户连接
