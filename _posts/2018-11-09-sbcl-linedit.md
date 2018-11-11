---
layout: post
title:  sbcl 启用命令行补全
tags: [sbcl, commonlisp, linedit , quicklisp]
author: Luhux
comment: true
---


# 安装linedit

    $ sbcl
    sbcl> (ql:quickload "linedit")

在~/.sbclrc中加入

    (ql:quickload "linedit")
    (linedit:install-repl :wrap-current t :eof-quits t)


# 安装rlwarp

    # apt install rlwarp

# 启动

    $ rlwarp sbcl
    This is SBCL 1.4.12, an implementation of ANSI Common Lisp.
    More information about SBCL is available at <http://www.sbcl.org/>.
    SBCL is free software, provided as is, with absolutely no warranty.
    It is mostly in the public domain; some portions are provided under
    BSD-style licenses.  See the CREDITS and COPYING files in the
    distribution for more information.
    To load "linedit":
      Load 1 ASDF system:
        linedit
    ; Loading "linedit"
    ....

    Linedit version 0.17.6, smart mode, ESC-h for help.
    *


# 测试

    $ rlwarp sbcl
    sbcl> (for  按下tab

显示format 补全
安装成功

# 以后的使用

    $ rlwarp sbcl
