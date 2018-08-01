---
layout: post
title: 扩大镜像及分区
tags: [diskimage, linux]
author: luhux
comment: true
---

扩容镜像:

```
# ls -lh Armbian_5.35_Orangepilite_Ubuntu_xenial_default_3.4.113.img
-rw-r--r-- 1 root root 1.4G Nov 25  2017 Armbian_5.35_Orangepilite_Ubuntu_xenial_default_3.4.113.img
```
镜像默认1.4G

扩容128M: (请按需扩容)

```
# dd bs=1M count=128 if=/dev/zero of=128M.img  # 生成一个128M的空镜像 (count=256 创建256Mb)
128+0 records in
128+0 records out
134217728 bytes (134 MB, 128 MiB) copied, 1.37621 s, 97.5 MB/s
# cat 128M.img >> Armbian_5.35_Orangepilite_Ubuntu_xenial_default_3.4.113.img  # 为镜像追加128M空白空间
```
扩大分区:

```
# parted Armbian_5.35_Orangepilite_Ubuntu_xenial_default_3.4.113.img
GNU Parted 3.2
Using /usr/image/Armbian_5.35_Orangepilite_Ubuntu_xenial_default_3.4.113.img
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) unit   # 切换单位
Unit?  [compact]? MB    #  mb
(parted) print       # 打印分区信息
Model:  (file)
Disk /usr/image/Armbian_5.35_Orangepilite_Ubuntu_xenial_default_3.4.113.img: 1590MB  # 1590MB为扩容后的镜像大小
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags: 

Number  Start   End     Size    Type     File system  Flags
 1      4.19MB  1455MB  1451MB  primary  ext4                # 分区 1455MB

(parted) resizepart  # 扩大分区
Partition number? 1          # 第一分区
End?  [1455MB]? 1590                # 新分区大小不可超过镜像大小
(parted) print                  # 查看是否成功
Model:  (file)
Disk /usr/image/Armbian_5.35_Orangepilite_Ubuntu_xenial_default_3.4.113.img: 1590MB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags: 

Number  Start   End     Size    Type     File system  Flags
 1      4.19MB  1590MB  1585MB  primary  ext4  # 1590M 成功

```
挂载调整分区大小
```
# kpart -a -v Armbian_5.35_Orangepilite_Ubuntu_xenial_default_3.4.113.img # 挂载分区为块设备
add map loop0p1 (254:0): 0 3096576 linear /dev/loop 8192   # 245:0 指向镜像分区的块设备
# mkdir p1 # 创建挂载文件夹
# mount /dev/block/254\:0 p1  # 将镜像挂载至p1文件夹
# resize2fs /dev/block/254\:0  # 更新分区大小
```
卸载镜像
```
# sync  # 同步操作
# umount p1  # 卸载分区
# kpartx -d Armbian_5.35_Orangepilite_Ubuntu_xenial_default_3.4.113.img  # 卸载镜像
```

扩容完成
