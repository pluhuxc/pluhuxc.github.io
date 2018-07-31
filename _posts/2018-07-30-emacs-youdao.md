---
layout: post
title: Emacs使用有道翻译
tags: [emacs]
author: luhux
comment: true
---
项目主页:

[youdao-dictionary](https://github.com/xuchunyang/youdao-dictionary.el)

安装:

	M-x package-install
	youdao-dictionary
	
配置:

我的最简配置:

emacsinit文件添加:
```
;;youdao
(setq url-automatic-caching t)
(global-set-key (kbd "C-c y") 'youdao-dictionary-search-at-point)
(setq youdao-dictionary-search-history-file "~/.emacs.d/.youdao")
```

使用：

Ctrl + c y 翻译当前光标下的词

