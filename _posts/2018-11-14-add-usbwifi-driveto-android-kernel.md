---
layout: post
title: 在安卓内核中添加usbwifi驱动
tags: [linux, android, kernel]
author: Luhux
comment: true
---


在make menuconfig中勾选

```
[*] Networking support  --->
    -*-   Wireless  --->
         <*>   Generic IEEE 802.11 Networking Stack (mac80211)
```

```
    Device Drivers  --->
        [*] Network device support  --->
            [*]   Wireless LAN  --->
                < >   Marvell 8xxx Libertas WLAN driver support with thin firmware
                < >   Atmel at76c503/at76c505/at76c505a USB cards
                < >   Softmac Prism54 support (NEW)          
                < >   Ralink driver support (NEW)  --->         
                < >   Realtek wireless card support
                ....
```

将有需要的网卡勾选
并退出保存

编译内核请看:

https://pluhuxc.github.io/2018/11/13/costom-android-kernel.html

安装好内核后有一些硬件要有固件才可以使用:

在安卓机上执行:

    android # dmesg | grep bin

可以看出缺少的bin文件


缺少的bin文件可以去debian源内的 firmware- 软件包中下载

我这里打包了一份:


链接: https://pan.baidu.com/s/1nD9NiXwvEsORe0Kr8xGxFQ 提取码: dbcx 

找到缺少的bin文件并拷贝到 安卓机的 /system/etc/firmware

权限不可写的话:

    android # mount -o remount,rw /system


