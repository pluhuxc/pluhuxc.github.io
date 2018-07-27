---
layout: post
title: Install the slackwarearm on android
tags: [android, slackware, linux, arm]
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

<script src="https://asciinema.org/a/5S1rj4kDF3ljuf14OG8POQVex.js" id="asciicast-5S1rj4kDF3ljuf14OG8POQVex" async></script>

2. 下载rootfs

### 命令前缀表示命令执行环境:
```
$ 代表adb客户端机器

android $ 代表安卓普通用户终端

android # 代表安卓root用户终端

bash # 代表slackware未初始化终端

root@localhost # 代表chrootslackware终端
```

```
android $ su  # 切换安卓root终端
android # cd /data  # 切换到data分区
android # mkdir slackware  # 创建安装目录
android # wget http://ftp.arm.slackware.com/slackwarearm/slackwarearm-devtools/minirootfs/roots/slack-current-miniroot_01Jul18.tar.xz  # 下载slackwrearmrootfs
```
<script src="https://asciinema.org/a/S9IvNjAoi4BNhTBMzsXWl9iup.js" id="asciicast-S9IvNjAoi4BNhTBMzsXWl9iup" async></script>

3. 解包rootfs

```
android # cd /data/slackware  # 切换到安装目录
android # xz -d slack-*  # 解包rootfs
android # tar xf slack-*  # 解包 rootfs
```
<script src="https://asciinema.org/a/vD9JHEd9n4A4PZIj7p6l5lOFK.js" id="asciicast-vD9JHEd9n4A4PZIj7p6l5lOFK" async></script>

4. 配置
```
android # cd /data/slackware/ 
android # rm -rf /data/slackware/dev/*  # 删除无用文件
android # cp /etc/hosts /data/slackware/etc/  # 复制安卓hosts文件
android # cp /etc/resolv.conf /data/slackware/etc/  # 复制配置文件 
android # mount -o bind /dev/ dev  # 挂载dev 到rootfs
android # mount -o bind /dev/pts dev/pts # 挂载dev/pts 到rootfs
android # mount -o bind /sys/ sys   # 挂载sys到rootfs
android # mount -o bind /proc/ proc  # 挂载proc到rootfs
android # unset LD_PRELOAD  # 取消当前终端 LD_PRELOAD 变量
```
<script src="https://asciinema.org/a/vs0uy6nU6VXP7JR5BVYKLjIt6.js" id="asciicast-vs0uy6nU6VXP7JR5BVYKLjIt6" async></script>
```
android # unset LD_PRELOAD  # 取消当前终端 LD_PRELOAD 变量
android # chroot /data/slackware  # chroot 进入slackware
bash # source /etc/profile  # 初始化bash
root@localhost # useradd -s /bin/false -u 3003 -M aid_inet  # 添加aid_inet权限组
root@localhost # useradd -s /bin/fasle -u 3004 -M aid_net_raw  # 添加aid_net_raw权限组
root@localhost # groupmod -g 3003 aid_inet  # 改用户组guid
root@localhost # groupmod -g 3004 aid_net_raw  # 改用户组guid
root@localhost # usermod -G aid_inet root  # 将root用户添加至socket权限组
root@localhost # usermod -G aid_net_raw root  # 将root用户添加至raw_socket权限组
```
<script src="https://asciinema.org/a/CkvhBa4ezzU1OKp8n03nZx1l7.js" id="asciicast-CkvhBa4ezzU1OKp8n03nZx1l7" async></script>

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
<script src="https://asciinema.org/a/9NvByMZYme8ACc9b2bdBTRW1q.js" id="asciicast-9NvByMZYme8ACc9b2bdBTRW1q" async></script>

## 如果你要使用需要套接字权限的应用程序

你需要将运行用户添加至套接字权限组

例如:

xrdp:

    usermod -G aid_inet xrdp

mysql:

    usermod -G aid_inet mysql

5. 安装完成之后:

创建启动脚本:
```
android # cd /data/slackware
android # touch start.sh
android # chmod 0755 start.sh
android # echo '#!/system/bin/sh' >> start.sh 
android # echo 'cd /data/slackware' >> start.sh
android # echo 'mount -o bind /dev/ dev' >> start.sh
android # echo 'mount -o bind /dev/pts dev/pts' >> start.sh
android # echo 'mount -o bind /sys/ sys' >> start.sh 
android # echo 'mount -o bind /proc/ proc' >> start.sh
android # echo 'unset LD_PRELOAD' >> start.sh 
android # echo 'chroot /data/slackware' >> start.sh 
```
<script src="https://asciinema.org/a/XQ79dGlU24DuOut5o74xZbjLQ.js" id="asciicast-XQ79dGlU24DuOut5o74xZbjLQ" async></script>
# 安装已完成

6. 使用slackware

启动ssh:

    android # cd /data/slackware
    android # sh start.sh #警告 必须使用前缀sh执行否则会导致变量全局导致安卓系统死机 
	bash # source /etc/profile
    root@localhost # /etc/rc.d/rc.sshd start



关闭系统并重启:

    android # reboot



# 当然,可以为它安装vnc,或者xrdp.

## 转载请注明

## 作者:luhux

## 邮箱: luhux@hotmail.com

![running](https://raw.githubusercontent.com/luhux/images/master/slackwarearmonandroid.png)
