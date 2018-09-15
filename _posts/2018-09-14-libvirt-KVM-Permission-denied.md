---
layout: post
title: livirt控制台的failed to initialize KVM: Permission denied错误解决
tags: [linux, kvm ,qemu,permission]
author: Luhux
comment: true
---

安装虚拟机报错:
```
ternal error: qemu unexpectedly closed the monitor: Could not access KVM kernel module: Permission denied
failed to initialize KVM: Permission denied'
```


权限问题:

```
chown libvirt-qemu /dev/kvm
```

解决
