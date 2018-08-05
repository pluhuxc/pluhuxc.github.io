
---
layout: post
title: Emacs使用内置中文输入法
tags: [linux, emacs, im, chinese]
author: Luhux
comment: true
---
在Emacs 中发现了内建中文输入法

比pyim流畅很多.


启用:
在emacsinit文件加入:
```
(setq default-input-method "chinese-py")
;; chinese-py为拼音输入法
(global-set-key (kbd "C-\\") 'toggle-input-method)
;; C - \ 切换至中文输入模式
```

这输入法真快

![EmacsBuildinchineseim](https://raw.githubusercontent.com/luhux/images/master/Emacs-buildin-im.png)
