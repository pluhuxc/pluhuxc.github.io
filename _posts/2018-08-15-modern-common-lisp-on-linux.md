---
layout: post
title: 为Emacs配置clisp
tags: [Emacs, linux, sbcl, clisp]
author: Luhux
comment: true
---

[modern-common-lisp-on-linux](http://www.jonathanfischer.net/modern-common-lisp-on-linux/)

## 安装sbcl:

### 包管理器安装:

#### Debian:

    $ sudo apt install sbcl

### 常规安装:

#### 下载:

[sbcl](http://www.sbcl.org/platform-table.html)

#### 安装:

    $ cd Downloads/
    $ tar xf sbcl-*
    $ cd sbcl-*
    $ sudo sh install.sh

### 验证安装:

	$ sbcl

## 安装quicklisp 和 slime:

[quicklisp](https://www.quicklisp.org/beta/)

### 安装quicklisp:

	$ wget https://beta.quicklisp.org/quicklisp.lisp
	$ sbcl --load quicklisp.lisp
	
#### 进入sbcl * 提示符后:

	* (quicklisp-quickstart:install)
	* (ql:add-to-init-file)
   
执行完毕后不要退出sbcl

### 安装slime:

	* (ql:quickload "quicklisp-slime-helper")
	
## 配置Emacs:

在Emacsinit文件中加入:

```
(setq inferior-lisp-program "sbcl")
(load (expand-file-name "~/quicklisp/slime-helper.el"))
```

# 验证:

在Emacs中:

	M-x slime RET
	
等待出现slime提示符:

	CL-USER>
	
成功
