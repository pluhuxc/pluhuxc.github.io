---
layout: post
title: 裁剪树梅派镜像
tags: [linux, sd, image, raw, raspi]
author: Luhux
comment: true
---

# 教程使用官方镜像做演示

使用dd备份出来的镜像太大 8个g  所以需要将使用的部分搬迁到一个较小的新镜像里面 以支持小内存卡和节省时间


思路:
```
+--------+
| 原镜像 | 
+--------+ 
   |
loop挂载
   |
挂载到目录
   |
计算新镜像大小 ---- 制作新镜像 --- 创建分区表和分区 
	                                        |
                                         loop挂载
										    |
   --- 	同步（复制）原文件到新镜像 ---  挂载到目录
   |
修改与分区相关的文件
   | 
  卸载分区
  卸载loop
   |
+--------+
| 新镜像 |
+--------+
```





因为树梅派官方镜像使用 

× mbr分区表
× 一个fat32的boot分区
× 一个任意类型的根分区

# 安装工具

	# apt install kpartx rsync 

# 挂载镜像

	$ ls raspbian.img
	$ sudo kpartx -av raspbian.img
	
kpartx执行后会提示挂载点,我的为 /dev/mapper/loop0p1 和 p2

这里p1 为 vfat 的 boot 分区
这里p2 为 ext4 的 root 分区

创建挂载点

    # mkdir /mnt/loop0p1
	# mkdir /mnt/loop0p2
	
挂载分区

	# mount /dev/mapper/loop0p1 /mnt/loop0p1
	# mount /dev/mapper/loop0p2 /mnt/loop0p2
	

判断根分区使用情况:

	# df -h
	
新镜像应大于根loop0p2的使用情况的300m左右

# 创建新镜像

	$ dd bs=1500m count=1 if=/dev/zero of=new_raspbian.img
	
这里创建了一个1500m的镜像:

× 128M 用来fat32 的 boot分区
× 剩余用来 根分区

分区新镜像:

	# parted new_raspbian.img --script -- mklabel msdos
	# parted new_raspbian.img --script -- mkpart primary fat32 0 128
	# parted new_raspbian.img --script -- mkpart primary ext4 128 -1

# 挂载新镜像

	# kpartx -av new_raspbian.img 
	
它输出新镜像的分区设备 我的是 /dev/mapper/loop1p1 和 /dev/mapper/loop1p2

格式化分区:

	# mkfs.vfat /dev/mapper/loop1p1   #将第一个分区格式化为fat32分区格式
	# mkfs.ext4 /dev/mapper/loop1p2   #将第二个分区格式化为ext4分区格式
	

创建挂载点:

	# mkdir /mnt/loop1p1
	# mkdir /mnt/loop1p2

挂载：

    # mount /dev/mapper/loop1p1 /mnt/loop1p1
    # mount /dev/mapper/loop1p2 /mnt/loop1p2
	
# 将原镜像复制搬迁到新镜像中

	# rsync -HPavz -q /mnt/loop0p2 /mnt/loop1p2
	# cp -r /mnt/loop0p1/* /mnt/loop1p1/
	
等待搬迁完成

如果提示目标空间不足请卸载后重新创建:

	# umount /mnt/loop1p1
	# umount /mnt/loop1p2
	# kpartx -dv /dev/loop1
	# losetup -d /dev/loop1
	
返回创建新镜像并重新创建


# 搬迁完成后


	# cat /mnt/loop1p2/etc/fstab
	
由于raspbian默认使用uuid来识别挂载点
所以搬迁后需要更改

	# vim /mnt/loop1p2/etc/fstab
	
将定义boot分区和 / 根分区的开头改为全路径设备（也可以根据新识别填写uuid）

如果是内存卡启动请改为 mmcblk0 的p1 和p2
如果是usb启动请改为 sda 的p1 和 p2

比如我使用内存卡改为这样

```
proc            /proc           proc    defaults          0       0
/dev/mmcblk0p1  /boot           vfat    defaults          0       2
/dev/mmcblk0p2  /               f2fs    defaults,noatime  0       1
# a swapfile is not a swap partition, no line here
#   use  dphys-swapfile swap[on|off]  for that
```

接下来要修改的是cmdline.txt 内核启动参数:


	# vim /mnt/loop1p1/cmdline.txt
	
将其中的 root=  后面的uuid改为绝对路径 （ 更改启动根分区）

更改方法和fstab第一项一样

比如我改为这样:

```
dwc_otg.lpm_enable=0 console=serial0,115200 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4 elevator=deadline fsck.repair=yes rootwait
```


修改完成.

# 卸载分区

	# sync
	# umount /mnt/loop1p1
	# umount /mnt/loop1p2
	# kpartx -dv /dev/loop1
	# losetup -d /dev/loop1
	# umount /mnt/loop0p1
	# umount /mnt/loop0p2
	# kpartx -dv /dev/loop0
	# losetup -d /dev/loop0
	
这样你就得到一个裁剪(裁剪掉0)版的镜像，写入树梅派测试是否可以启动
