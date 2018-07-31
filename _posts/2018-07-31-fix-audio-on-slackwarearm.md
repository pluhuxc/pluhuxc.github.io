---
layout: post
title: 解决 slackwarearm 在树梅派上无法输出音频
tags: [slackware, arm, linux, raspberrypi]
author: Luhux
comment: true
---

拿slackwarearm玩ONS时发现没有音频输出

修复:

	$ sudo -i
	# cd /boot
	# nano config.txt
	
找到:
```
# uncomment if audio is not working
# dtparam=audio=on
```
将dtparam=audio=on注释取消

```
dtparam=audio=on
```

重启

解决
