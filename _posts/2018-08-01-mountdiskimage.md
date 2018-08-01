---
layout: post
title: 挂载修改opilite安装镜像
tags: [arm, linux, raspberrypi]
author: Luhux
comment: true
---

armbian:
[armbian](http://dl.armbian.com/)

```
# kpart -a -v Armbian_5.35_Orangepilite_Ubuntu_xenial_default_3.4.113.img # 挂载镜像内分区为块设备
add map loop0p1 (254:0): 0 3096576 linear /dev/loop 8192   # 245:0为指向镜像分区的块设备
# mkdir p1 # 创建挂载文件夹
# mount /dev/block/254\:0 p1  # 将镜像挂载至p1文件夹
```
镜像已挂载至p1文件夹下

你现在可以对镜像内的文件进行编辑（例如: 编辑网络配置文件,添加用户)

在一切操作完毕后:
```
# sync  # 同步操作
# umount p1  # 卸载分区
# kpartx -d Armbian_5.35_Orangepilite_Ubuntu_xenial_default_3.4.113.img  # 卸载镜像
```

