---
layout: post
title: 简单使用lxc容器
tags: [lxc, container,linux]
author: Luhux
comment: true
---


# 安装

	# apt install lxc
	
# 设置

	# service start lxc     # 测试lxc是否可以初始化

	# systemctl enable lxc   # 加入开机启动项
	
# 创建容器

	# lxc-create -n 容器名 -t 模板名 
	
* 模板目录 /usr/share/lxc/templates/

比如创建一个Debian容器

	# apt install debootstrap    # 创建容器内系统需要的工具
	
	# lxc-create -n debian00 -t debian

等待创建完成

# 配置容器



	# cd /var/lib/lxc/容器名/rootfs
	
	# vim ./etc/ssh/sshd_config     # 修改ssh默认端口和允许root登录
		
加入或修改
	
``` 
#Port 22
Port 3322   # 修改默认监听端口为3322

#PermitRootLogin prohibit-password
PermitRootLogin yes
```

	# lxc-exectue -n 容器名 passwd    # 设置容器内root密码
	
## 配置网络连接方式

	# vim /var/lib/lxc/容器名/config
	
加入或修改

```
#lxc.network.type = empty
lxc.network.type = none    # 与外部共享网络命名空间
```


# 启动

	# lxc-start -n 容器名
	
# 连接

	# ssh -p 3322 root@localhost    # 通过ssh连接


# 启动失败

一些解决方案


* 使用htop追踪启动过程 查看无法启动的服务, 尝试禁用或修复

* 移除或修改容器内与外部服务(共用网络命名空间) 端口冲突的项
