---
layout: post
title: 改变系统启动级别
tags: [linux, inittab , gui , tty, termianl]
author: Luhux
comment: true
---


编辑/etc/inittab文件

```
# Default runlevel. (Do not set to 0 or 6)
id:3:initdefault:
```

* id:num:initdefault:中的num表示默认运行级别

修改开机默认模式请修改num为以下数字:

| num | 描述                   |
|-----|------------------------|
| 1   | 单用户模式             |
| 2   | 未设定模式,与3模式相同 |
| 3   | 多用户名令行模式, 默认不开机自启图形请选择 |
| 4   | 登录管理器模式,默认开机自启图形请选择 |
| 5   | 空模式,与3模式相同|


例如:

开机自动进入TTY:

```
# Default runlevel. (Do not set to 0 or 6)
id:3:initdefault:
```

开机自动进入登录管理器:

```
# Default runlevel. (Do not set to 0 or 6)
id:4:initdefault:
```


修改保存 
