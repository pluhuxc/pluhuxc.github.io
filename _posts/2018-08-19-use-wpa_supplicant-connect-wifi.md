---
layout: post
title: 使用wpa_supplicant手动配置连接wifi
tags: [wpa_supplicant, wifi, network, linux]
author: luhux
comment: true
---

使用前请卸载掉NetowrkManager,wicd等网络管理工具

## wpa配置生成器创建框架

	# wpa_passphrase "<wifi_ssid>" "<password>" >> /etc/wpa_supplicant.conf

* \<wifi_ssid\>为wifi名 

* \<password\> 为密码 

* \>\> /etc/wpa_supplicant.conf 为将输出追加至/etc/wpa_supplicant.conf

例如:
wifi名: Tenda_80211

密码: I love linux

	# wpa_passphase "Tenda_80211" "I love linux" >> /etc/wpa_suppliant.conf

## 修改配置

	# nano /etc/wpa_supplicant.conf

找到以下行

```
network={
	ssid="wifi_ssid"
	#psk="密吗"
	psk=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
}
```

例子:

```
network={
	ssid="Tenda_80211"  # wifi名
	#psk="I love linux"  # 密码
	psk=01d37dde39f4cdca60863afeb133f5f6b892ca45dd9c190586bea58141260274  # 加密后的密码
}
```
	
* ssid="" 为要连接的ssid

* psk="" 为wifi密码 可选加密后也可明文

#### 如果不想泄漏wifi密码则:

* 删除明文密码行

* 对配置文件设置 0600 权限


## *连接隐藏ssid的wfi
```
network={
	ssid="Tenda_80211"
	#psk="I love linux"
	psk=01d37dde39f4cdca60863afeb133f5f6b892ca45dd9c190586bea58141260274
}
```
加入:
```
	scan_ssid=1
```
就像这样:
```
network={
	ssid="Tenda_80211"
	scan_ssid=1
	#psk="I love linux"
	psk=01d37dde39f4cdca60863afeb133f5f6b892ca45dd9c190586bea58141260274
}
```

# 修改完成后保存

## 测试连接配置

	# ifconfig wlan0 up
	
开启网卡

	# wpa_supplicant -D nl80211 -i wlan0 -c /etc/wpa_supplicant.conf &
	
* -D 指定网卡驱动可为: nl80211 wext 

* -i 指定网卡 

* -c 指定配置文件

* & 挂起在后台

### dhcp:

	# dhclient
	
dhcp获取ip地址

### 如果静态ip请:


	# ifconfig wlan0 192.168.1.119 netmask 255.255.255.0
	# route add default gw 192.168.1.1
	
自行更改:

* 192.168.1.119 为要设置的静态ip

* 255.255.255.0 为要设置的子网掩码

* 192.168.1.1 为要设置的网关

无法上网请自行更改dns

## 如果无法连接:

* kill 掉 wpa_supplicant 和 dhclient 的进程 以重新测试

* 检查网卡名

* 检查配置文件

* -D更换驱动


## 测试完成后:

加入开机脚本:

rc.local:

	# touch /etc/rc.local
	# chmod +x /etc/rc.local
	
如果脚本为空:

加入:

```
#!/bin/sh


exit 0
```

### 写入开机连接命令

在exit 0之前写

```
ifconfig wlan0 up
wpa_supplicant -B -D nl80211 -i wlan0 -c /etc/wpa_supplicant.conf
dhclient 
```

* -B 参数为后台Daemon运行

命令请自行更改为测试通过的命令

重启:


	# ifconfig 
	
查看ip

	# ping www.baidu.com
	
查看是否可以联网
	
