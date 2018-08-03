---
layout: post
title: 在linux上创建一个ramdisk
tags: [ramdisk, tmpfs , linux]
author: luhux
comment: true
---

建议内存盘大小别超过内存的百分之80

```
# mkdir /ramdisk  # 创建挂载点
# mount -t tmpfs tmpfs /ramdisk -o size=256M  # 创建一个256M的内存盘挂载到/ramdisk
```
