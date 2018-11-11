---
layout: post
title: iptables设置对外白名单
tags: [iptbales, drop , linux]
author: Luhux
comment: true
---

# 初始化

    # iptables -P OUTPUT DROP

将所有对外连接丢弃


# 添加允许访问的ip

    # iptables -A OUTPUT -d 允许访问的IP -j ACCEPT


# 永远应用


将iptables的命令添加到开机脚本:

例如只允许访问百度：


     iptables -P OUTPUT DROP
     iptables -A OUTPUT -d 111.13.100.91 -j ACCEPT

