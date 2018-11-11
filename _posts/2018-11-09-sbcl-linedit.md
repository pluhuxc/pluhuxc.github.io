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

    ;;; Check for --no-linedit command-line option.
    (if (member "--no-linedit" sb-ext:*posix-argv* :test 'equal)
        (setf sb-ext:*posix-argv* 
          (remove "--no-linedit" sb-ext:*posix-argv* :test 'equal))
        (when (interactive-stream-p *terminal-io*)
          (require :sb-aclrepl)
          (require :linedit)
          (funcall (intern "INSTALL-REPL" :linedit) :wrap-current t)))

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
    sbcl>

# 测试

    $ rlwarp sbcl
    sbcl> (for  按下tab

显示format 补全
安装成功

# 以后的使用

    $ rlwarp sbcl


# 不影响slime 和 sly的使用

在.emacs加入

    (setq inferior-lisp-program "sbcl --noinform --no-linedit") 
