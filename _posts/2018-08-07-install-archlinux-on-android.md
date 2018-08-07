---
layout: post
title: 在安卓上安装archlinux
tags: [android, archlinux, linux, arm]
author: luhux
comment: true
---

### 需要安卓的root权限

*为非必需

1. adb连接手机*

打开安卓手机连接和adb客户端机器(安装有adb的机器)连接同一网络
打开安卓手机的网络adb

![Android adb](https://raw.githubusercontent.com/luhux/images/master/Android_adb.png)

    $ adb connnect ip地址
	$ adb shell 

2. 下载rootfs

### 命令前缀表示命令执行环境:
```
$ 代表adb客户端机器
android $ 代表安卓普通用户终端
android # 代表安卓root用户终端
bash # 代表archlinux未初始化终端
root@localhost # 代表chrootarchlinux终端
```

请使用archlinux源内最新rootfs链接替换

[rootfs](http://mirrors.ustc.edu.cn/archlinuxarm/os/)

```
android $ su  # 切换安卓root终端
android # cd /data  # 切换到data分区
android # mkdir archlinux  # 创建安装目录
android # cd archlinux 
android # wget http://mirrors.ustc.edu.cn/archlinuxarm/os/ArchLinuxARM-armv7-latest.tar.gz  # 下载ubunturootfs
```
3. 解包rootfs

```
android # cd /data/archlinux  # 切换到安装目录
android # xz -d ArchLinuxARM*  # 解包rootfs
android # tar xf ArchLinuxARM*  # 解包 rootfs
```


4. 配置
```
android # cd /data/archlinux/ 
android # rm -rf /data/archlinux/dev/*  # 删除无用文件
android # cp /etc/hosts /data/archlinux/etc/  # 复制安卓hosts文件
android # cp /etc/resolv.conf /data/archlinux/etc/  # 复制配置文件 
android # mount -o bind /dev/ dev  # 挂载dev 到rootfs
android # mount -o bind /dev/pts dev/pts # 挂载dev/pts 到rootfs
android # mount -o bind /sys/ sys   # 挂载sys到rootfs
android # mount -o bind /proc/ proc  # 挂载proc到rootfs
android # unset LD_PRELOAD  # 取消当前终端 LD_PRELOAD 变量
```

```
android # unset LD_PRELOAD  # 取消当前终端 LD_PRELOAD 变量
android # chroot /data/archlinux /bin/bash  # chroot 进入archlinux
bash # source /etc/profile  # 初始化bash 会报cat命令错误(正常)
root@localhost # useradd -s /bin/false -u 3003 -M aid_inet  # 添加aid_inet权限组
root@localhost # useradd -s /bin/fasle -u 3004 -M aid_net_raw  # 添加aid_net_raw权限组
root@localhost # groupmod -g 3003 aid_inet  # 改用户组guid
root@localhost # groupmod -g 3004 aid_net_raw  # 改用户组guid
root@localhost # usermod -G aid_inet root  # 将root用户添加至socket权限组
root@localhost # usermod -G aid_net_raw root  # 将root用户添加至raw_socket权限组
```


## 更改root用户密码
```
root@localhost # passwd
```
## 添加新用户

```
root@localhost # adduser user # user换成用户名
root@localhost # usermod -G aid_inet user # 使用户加入socket权限组
root@localhost # usermod -G aid_net_raw user  # 使用户加入socket_raw权限组
```

## 如果你要使用需要套接字权限的应用程序

你需要将运行用户添加至套接字权限组

例如:

xrdp:

    usermod -G aid_inet xrdp

mysql:

    usermod -G aid_inet mysql

5. 安装完成之后:

创建启动脚本:
使用androidroot权限shell执行

```
android # cd /data/archlinux
android # touch start.sh
android # chmod 0755 start.sh
android # echo '#!/system/bin/sh' >> start.sh 
android # echo 'cd /data/archlinux' >> start.sh
android # echo 'mount -o bind /dev/ dev' >> start.sh
android # echo 'mount -o bind /dev/pts dev/pts' >> start.sh
android # echo 'mount -o bind /sys/ sys' >> start.sh 
android # echo 'mount -o bind /proc/ proc' >> start.sh
android # echo 'unset LD_PRELOAD' >> start.sh 
android # echo 'chroot /data/archlinux /bin/bash' >> start.sh 
```

# 安装已完成

6. 使用archlinux

启动ssh:

    android # cd /data/archlinux
    android # sh start.sh #警告 必须使用前缀sh执行否则会导致变量全局导致安卓系统死机 
	bash # source /etc/profile
    root@localhost # /etc/rc.d/rc.sshd start



关闭系统并重启:

    android # reboot



# 当然,可以为它安装vnc,或者xrdp.

## 转载请注明

## 作者:luhux

## 邮箱: luhux@hotmail.com


