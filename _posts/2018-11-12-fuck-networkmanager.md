---
layout: post
title: 个人对network-manager和/etc/interface文件的看法
tags: [linux, network ]
author: Luhux
comment: true
---

# /etc/interface 网络管理文件

缺点：

* 在各个linux發行上有不同版本，没有一个标准的一种,不通用

# NetworkManager 网络管理器

缺点：

* 没有我想要的许多功能，配置繁琐。
* 和许多我要用到的自定义功能冲突


# 解决

弃用NetworkManager自己写了网络初始化脚本和各个网卡的管理脚本

