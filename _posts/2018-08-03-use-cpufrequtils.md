---
layout: post
title: 使用cpufrequtils查看调整cpu频率及模式
tags: [linux, cpufreq, cpu]
author: luhux
comment: true
---

debian安装:

	# apt install cpufrequtils

使用:

### cpufreq-info  查看当前cpu状态

参数:

|参数 |值   |说明|
|----|-----|----|
|-c |CPU序号| 查看所指定cpu状态|
|-f |  |     查看cpu当前频率|
|-l    |  |  查看cpu最高频率和最低频率|
|-p     |  | 查看当前cpu模式|
|-g      | |  查看当前支持的CPU运行模式|
|-m      | | 带单位的输出|

### cpufreq-set 设置cpu模式及频率

| 参数 | 值 | 说明 |
| ---- | ----- | ---- |
| -c | CPU序号 | 设置修改指定cpu |
| -d | 频率 | 设置cpu最小运行频率 |
| -u | 频率 |    设置cpu最大运行频率 |
| -g | 模式 |   设置cpu模式 |

频率支持单位:

Hz kHz MHz GHz

常用CPU模式:

| 模式 | 说明 |
| ---- | ----| 
|powersave|以最低频率运行cpu|
|ondemand|程序运行时切换cpu频率到最高,不使用时降低到最低|
|performance|最高性能模式,以最高频率运行cpu|

设置后会马上生效

重启后会失效

可以自己编辑/etc下的开机启动脚本设置开机自动调整CPU
