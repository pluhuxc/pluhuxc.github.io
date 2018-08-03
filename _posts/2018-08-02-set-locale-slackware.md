---
layout: post
title: slackware设置locale
tags: [slackware, linux, locale]
author: luhux
comment: true
---

系统全局设置:

```
# vi /etc/profile.d/lang.sh
```
更改:

```
export LANG=en_US.UTF-8
```
  |
  V
```
export LANG=zh_CN.utf8
```

局部用户设置:

gui:
```
$ vi ~/.xinitrc
```
加入:
```
export LANG=zh_CN.utf8
```
terminal:
```
$ vi ~/.bashrc
```
加入
```
export LANG=zh_CN.utf8
```

