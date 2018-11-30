---
layout: post
title: 自行编译安卓内核
tags: [linux, android, kernel , driver]
author: Luhux
comment: true
---

--------
1. 确定设备信息
2. 寻找设备源码
3. 安装对应的工具链
4. 初始化环境
5. config文件
6. make menuconfig
7. make -j4
8. 把编译出来的文件拷贝出来
9. 提取设备boot.img
10. 解包设备boot.img
11. 替换原内核
12. 打包boot.img
13. 刷入
14. 测试
15. 作者唠几句
16. 感谢名单


# 确定设备信息

## 获取设备代号


在安卓设备上执行:

    android # getprop | grep device

并寻找带有 ro.xx.device这一行

如果是cm它为：

    [ro.cm.device]: [设备代号]

我的设备：

我的设备是红米2,系统为rros:

 
    android # getprop | grep device
    [ro.boot.bootdevice]: [7824900.sdhci]
    [ro.device.cache_dir]: [/cache]
    [ro.device.chipset]: [Qualcomm MSM8916 Snapdragon 410]
    [ro.device.cpu]: [Quad-core 1.2 GHz Cortex-A53]
    [ro.device.front_cam]: [2 MP]
    [ro.device.gpu]: [Adreno 306]
    [ro.device.rear_cam]: [8 MP]
    [ro.device.screen_res]: [720x1280]
    [ro.device_owner]: [false]
    [ro.frp.pst]: [/dev/block/bootdevice/by-name/config]
    [ro.opa.eligible_device]: [true]
    [ro.product.device]: [HM2014811]
    [ro.rr.device]: [wt88047]


可以看出最后一行为我的设备代号:


    [ro.rr.device]: [wt88047]


## 获取设备架构

在安卓设备上执行:

    android # uname -m

如果是armv7设备它会显示为:

    armv7

如果是arm64设备它会显示为:

    armv8

我的安卓设备为红米2 


    android # uname -m
    armv7l
 

可以看出我的设备为armv7设备

## 获取设备内核版本

在安卓设备上执行:

    android # uname -r 

它会输出：

    版本.版本.版本-内核自定义字符串


我的设备为红米2,rros:


    android # uname -r
    3.10.107-ImmortalKernel


可以看出我的内核版本为 3.10.107

# 寻找设备源码

## 在github上

这里我直接拿我的红mi2举例 ， 请看举例

### 在LineageOS寻找

LineageOS 的仓库地址:

https://github.com/LineageOS/

在仓库的搜索栏中输入:

    kernel

并搜索

并在搜索出来的仓库名中寻找你的设备

    android_kernel_设备厂商_cpu代号

比如我的红米2，厂商为小米， 设备代号为 wt88047 , cpu是msm8916(高通晓龙410)

但是我设备有点特殊，我设备的实际厂商为wingtech并不是XiaoMi

所以我找到了我的设备源码仓库:

    https://github.com/LineageOS/android_kernel_wingtech_msm8916


确认内核版本:

* 内核版本离当前内核越近越好，版本差别太大的内核可能导致各种问题

查看源内的Makefile前几行:


    VERSION = 3
    PATCHLEVEL = 10
    SUBLEVEL = 108


看出了:

这个源码版本为:

    3.10.108

我设备的为3.10.107

寻找属于我设备的config文件

在仓库内寻找:

    arch/你设备的架构/configs/lineageos_你设备架构_defconfig

我的设备为红米2 架构为armv7 代号为wt88047

    arch/arm/configs/lineageos_wt88047_defconfig

找到!

很符合我的设备，所以使用

### 下载源码：

点 clone or download -> download zip

### 如果未找到符合的怎么办？

* 去论坛找源码

* 切换搜索方式试试

* 全github搜索

### 源码是找到了.可是版本差距太大怎么办

* 去切换当前仓库的分支 

* 就拿这个凑活用 

* 全github搜索

# 安装对应工具链

建议安装4.9.x的gcc工具链

如果是armv7设备:

    arm-linux-gnueabihf-

如果是arm64设备:

    aarch64-linux-gnu-

x86 64的机器可以去linaro下载:

    https://releases.linaro.org/components/toolchain/binaries/4.9-2017.01/

下载后解压到~/.local/

解压后看起来就像这样

我的设备armv7 用的 arm-linux-gnueabihf-


    linux $ ls ~/.local/gcc-linaro-4.9.4-2017.01-x86_64_arm-linux-gnueabihf/
    arm-linux-gnueabihf  bin  include  lib  libexec  share


* 如果你编译内核的机器就是arm那么你直接apt或者yum安装一个gcc-arm-linux-gnueabihf 或者 gcc-aarch64-linux-gnu

# 初始化环境

## 添加工具链到环境变量

记住你解压后的工具链目录名和路径

在工具链目录创建一个脚本来将工具链添加到PATH

    linux $ touch ~/.local/你工具链目录/gccenv.sh
    linux $ vim ~/.local/你工具链目录/gccenv.sh

添加:


    export PATH="${PATH}:${HOME}/.local/你的工具链目录/bin"



并保存关闭
    
比如我的linaro arm-linux-gnueabihf 工具链:


    linux $ touch ~/.local/gcc-linaro-4.9.4-2017.01-x86_64_arm-linux-gnueabihf/gccenv.sh
    linux $ vim ~/.local/gcc-linaro-4.9.4-2017.01-x86_64_arm-linux-gnueabihf/gccenv.sh

加入:

    export PATH="${PATH}:${HOME}/.local/gcc-linaro-4.9.4-2017.01-x86_64_arm-linux-gnueabihf/bin"


保存关闭

## 创建工作目录

    $ mkdir ~/android_kernel_work
    $ cd ~/android_kernel_work

将下载的源码解压并拷贝到此目录

我的看起来是这样:

    linux $ tree -L 2
    .
    ├── android_kernel_wingtech_msm8916-lineage-16.0
    │   ├── android
    │   ├── AndroidKernel.mk
    │   ├── arch
    │   ├── block
    │   ├── COPYING
    │   ├── CREDITS
    │   ├── crypto
    │   ├── Documentation
    │   ├── drivers
    │   ├── env.sh
    │   ├── firmware
    │   ├── fs
    │   ├── include
    │   ├── init
    │   ├── ipc
    │   ├── Kbuild
    │   ├── Kconfig
    │   ├── kernel
    │   ├── lib
    │   ├── MAINTAINERS
    │   ├── Makefile
    │   ├── mm
    │   ├── net
    │   ├── README
    │   ├── REPORTING-BUGS
    │   ├── samples
    │   ├── scripts
    │   ├── security
    │   ├── sound
    │   ├── tools
    │   ├── usr
    │   └── virt


## 设置编译变量

创建初始化编译变量的脚本

    $ touch ~/android_kernel_work/env.sh
    $ vim ~/android_kernel_work/env.sh

并加入:


    export ARCH=你的安卓设备架构
    export CROSS_COMPILE=你的工具链前缀


保存退出

比如我的红米2 设备架构为 armv7 使用 arm-linux-gnueabihf-

    $ touch ~/android_kernel_work/env.sh
    $ vim ~/android_kernel_work/env.sh


加入


    export ARCH=arm
    export CROSS_COMPILE=arm-linux-gnueabihf-

保存并退出

## 应用环境变量:

    linux $ source ~/.local/你的工具链目录/gccenv.sh
    linux $ source ~/android_kernel_work/env.sh

我的设备是红米2工具链目录为 gcc-linaro-4.9.4-2017.01-x86_64_arm-linux-gnueabihf :所以

    linux $ source ~/android_kernel_work/env.sh
    linux $ source ~/.local/gcc-linaro-4.9.4-2017.01-x86_64_arm-linux-gnueabihf/gccenv.sh

## 检查源码目录

    linux $ cd ~/android_kernel_work/你的源码目录/
    linux $ make clean
    linux $ make distclean
    

我的:

    linux $ cd ~/android_kernel_work/android_kernel_wingtech_msm8916-lineage-16.0/
    linux $ make clean
    linux $ make distclean
 

这样就创造了一个干净的编译目录

# config文件

## 从设备提取

在安卓设备执行

    android # ls /proc/config.gz

如果存在的话使用cp将config.gz复制出来拷贝到电脑

    android # cp /proc/config.gz /sdcard/config.gz


并拷贝到源码目录下面命名为 .config

像这样:

    ~/android_kernel_work/android_kernel_wingtech_msm8916-lineage-16.0/.config


如果不存在的话使用源码中的config文件

## 使用源码中的

    linux $ cp ~/android_kernel_work/你的源码目录/arch/设备架构/configs/你的设备_defconfig ~/android_kernel_work/你的源码目录/.config


比如我的红米2 设备代号wt88047 架构armv7
    
    linux $ cp ~/android_kernel_work/android_kernel_wingtech_msm8916-lineage-16.0/arch/arm/configs/lineageos_wt88047_defconfig ~/android_kernel_work/android_kernel_wingtech_msm8916-lineage-16.0/.config

# make menuconfig

一切初始化就绪，现在开始定制内核

    linux $ cd ~/android_kernel_work/你的源码目录/
    linux $ make menuconfig

在其中勾选你需要的功能和驱动

勾选完成后保存退出

我的：

    linux $ cd ~/android_kernel_work/android_kernel_wingtech_msm8916-lineage-16.0/
    linux $ make menuconfig

# make -j4

万事就绪，只欠编译

    linux $ cd ~/android_kernel_work/你的源码目录/
    linux $ make -j4 

* -j数字     代表编译的线程

我的:

    linux $ cd ~/android_kernel_work/android_kernel_wingtech_msm8916-lineage-16.0/
    linux $ make -j24

* 我的机器可以撑住24个线程同时编译


# 我曰，编译失败了....

* 检查config文件并返回初始化环境的make clean开始重新编译

* 检查源码并返回初始化环境的make clean开始重新编译

* 去掉不合理的config项并返回初始化环境的make clean开始重新编译

* 更换源码

# 把编译好的内核拷贝出来

编译完成后会在源码目录下的 arch/架构/boot/ 生成 zImage 文件

这个文件就是我们最终编译下来的内核


比如我的红米2 armv7:

    linux $ cp arch/arm/boot/zImage ~/


# 提取设备boot.img

在安卓设备执行

    android # dd if=/dev/block/bootdevice/by-name/boot of=/sdcard/boot.img

将boot.img拷贝到电脑一份，再在手机上留一份


# 解包设备boot.img


其实这个方法我是觉得十分方便，哈哈。

当然网上也有好多方法可以用


在安卓手机执行:

    android # mkdir /data/bootimg_work

下载magisk卡刷包到手机并移动到 

    /data/bootimg_work

进入工作目录:

    android # cd /data/bootimg_work/

解压卡刷包

    android # unzip Magisk-v17.1.zip

使用其中的解包打包二进制:


授予执行权限:

    android # chmod +x arm/*

将boot.img 拷贝到当前工作目录:

    android # cp /sdcard/boot.img ./boot.img

解包:

    android # ./arm/magiskboot --unpack boot.img

解包下来的kernel就是内核:

    android # ls -lh kernel

# 替换原内核

将编译好的zImage重新命名为kernel并拷贝到手机

并拷贝到工作目录替换原来的kernel文件

    android # cp /sdcard/kernel ./kernel

# 打包boot.img

在工作目录执行:

    android # ./arm/magiskboot --repack boot.img new-boot.img

这里的boot.img是原来的boot.img

这样的话生成了new-boot.img

将它拷贝到电脑    

# 刷入

重启手机并使用fastboot刷入

    linux $ fastboot flash boot new-boot.img

# 测试


刷入成功后重启
并测试你要的功能

如果刷入失败或者启动失败，请重新进入fastboot刷入原来备份出来的boot.img(恢复原来内核)


# 我说几句


总之就是看网上没有一个像样的安卓内核编译教程，自己心血来潮写了一个编译指南。提供给需要的人吧。

你要编译进去usb网卡的话可以拿otg 接 usb网卡和aircrack-ng配合抓包，或者攻击wpawifi，哈哈，

你要是再把kali-netunder的内核patch打进去的话你就有一个名副其实的黑客手机内核了,

用法很多，自己想吧


# 感谢名单

感谢纯真大佬与我交流这个想法

感谢xiaomi开源

感谢github

感谢天空

感谢大地

感谢重力

感谢电力

感谢

...

作者: luhux
