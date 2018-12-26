---
layout: post
title: 自行制作一个树苺派64位系统
tags: [linux, raspberrypi ,arm64 ,kernel ,rootfs ,debian]
author: Luhux
comment: true
---



markdown 排版不理想


[github排版](https://github.com/pluhuxc/pluhuxc.github.io/blob/master/_posts/2018-12-18-make-a-raspberrypi64system.md)


1. 准备环境

2. 编译内核

3. 创建镜像模板

4. 制作rootfs

5. 打包进镜像

6. 测试


# 树梅派64位系统ARCH是aarch64 或 arm64


# 准备环境

环境：

--------------

	Linux x86 或 Linux x86_64
	5G硬盘空间
	网络

--------------


## 格式

32位64位是指x86架构

树梅派架构一般标识为arm64或aarch64

```
$ 指普通用户
# 指root账户
```


## 编译内核用的工具链 gcc-4.9

Linaro下载:

https://releases.linaro.org/components/toolchain/binaries/

内核建议使用gcc 4.9的工具链构建

* 下载

64位：

	$ wget https://releases.linaro.org/components/toolchain/binaries/4.9-2017.01/aarch64-linux-gnu/gcc-linaro-4.9.4-2017.01-x86_64_aarch64-linux-gnu.tar.xz
	
32位:

	$ wget https://releases.linaro.org/components/toolchain/binaries/4.9-2017.01/aarch64-linux-gnu/gcc-linaro-4.9.4-2017.01-i686_aarch64-linux-gnu.tar.xz

* 解压

	$ mkdir ~/toolchains   # 创建目录

64位:

	$ tar xf gcc-linaro-4.9.4-2017.01-x86_64_aarch64-linux-gnu.tar.xz -C ~/toolchains/

32位:

	$ tar xf gcc-linaro-4.9-2017.01-i686_aarch64-linux-gnu.tar.xz -C ~/toolchains

* 创建工具链环境变量初始化脚本

64位：

	$ cd ~/toolchains/gcc-linaro-4.9.4-2017.01-x86_64_aarch64-linux-gnu/

32位：

	$ cd ~/toolchains/gcc-linaro-4.9.4-2017.01-i686_aarch64-linux-gnu/

创建环境变量脚本

	$ echo 'export PATH=${PATH}:$(pwd)/bin' > env.sh	

应用环境变量脚本

	$ source env.sh   # 将环境变量应用到当前终端


## 一些编译内核要用到的工具 build-essential bison make

	$ sudo apt install build-essential bison make

## 构建rootfs用的工具 debootstrap qemu-user-static

	$ sudo apt install debootstrap qemu-user-static

	$ sudo service binfmt-support start  # 启动binfmt服务

## 挂载镜像用到的工具 kpartx

	$ sudo apt install kpartx

## 分区镜像用到的工具 cfdisk

	$ sudo apt install util-linux

## 编辑配置文件编辑器 vim

	$ sudo apt install vim

## 树梅派内核

这里使用官方内核


创建工作目录

	$ mkdir kernel

进入工作目录

	$ cd kernel 

git clone 源码

	$ git clone https://github.com/raspberrypi/linux.git

克隆下来的文件夹在 ~/kernel/linux

## 树梅派boot文件

手动下载:

https://github.com/raspberrypi/firmware/tree/master/boot

一健下载

	$ git clone --depth=1 https://github.com/raspberrypi/firmware/ 

	$ cd firmware/boot   # 下载好的文件

## 树梅派wifi和蓝牙的固件

需要的固件文件


-------------

树梅派3b

WIFI:

``` text
brcmfmac43430-sdio.bin
brcmfmac43430-sdio.txt
```

Bluetooth:

```
BCM43430A1.hcd
```

---------------

树梅派3b+

WIFI:

``` text
brcmfmac43455-sdio.bin
brcmfmac43455-sdio.txt
```

Bluetooth:

```
BCM4345C0.hcd
```

-------------

建议从raspbian镜像内提取

现成raspbian的 /lib/firmware/brcm/ 文件夹


---------

网络获取方法:

手动下载

Wifi:

https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/brcm

Bluetooth:

https://github.com/RPi-Distro/bluez-firmware/tree/master/broadcom


# 编译内核

检查工具链

	$ which aarch64-linux-gnu-gcc

创建内核编译初始化脚本

	$ echo 'export ARCH=arm64' >> env.sh
	$ echo 'export CROSS_COMPILE=aarch64-linux-gnu-' >> env.sh

初始化环境

	$ source env.sh

检查环境变量

	$ echo "${ARCH}"

	$ echo "${CROSS_COMPILE}"

如果ARCH为arm64，CROSS\_COMPILE为aarch64-linux-gnu-则环境ok

进入刚才你准备好的内核源码目录

	$ cd ~/kernel/linux

## 使用默认配置

	$ make bcmrpi3_defconfig

## 修改默认配置

	$ make bcmrpi3_defconfig
	$ make menuconfig

如果menuconfig失败请安装libncurses-dev

	$ apt install libncurses5-dev

进入后的配置界面是这样的

``` text
 .config - Linux/arm64 4.14.89 Kernel Configuration
 ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ┌──────────────────────────────────────── Linux/arm64 4.14.89 Kernel Configuration ─────────────────────────────────────────┐
  │  Arrow keys navigate the menu.  <Enter> selects submenus ---> (or empty submenus ----).  Highlighted letters are hotkeys. │  
  │  Pressing <Y> includes, <N> excludes, <M> modularizes features.  Press <Esc><Esc> to exit, <?> for Help, </> for Search.  │  
  │  Legend: [*] built-in  [ ] excluded  <M> module  < > module capable                                                       │  
  │                                                                                                                           │  
  │ ┌───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐ │  
  │ │                            General setup  --->                                                                        │ │  
  │ │                        [*] Enable loadable module support  --->                                                       │ │  
  │ │                        [*] Enable the block layer  --->                                                               │ │  
  │ │                            Platform selection  --->                                                                   │ │  
  │ │                            Bus support  --->                                                                          │ │  
  │ │                            Kernel Features  --->                                                                      │ │  
  │ │                            Boot options  --->                                                                         │ │  
  │ │                            Userspace binary formats  --->                                                             │ │  
  │ │                            Power management options  --->                                                             │ │  
  │ │                            CPU Power Management  --->                                                                 │ │  
  │ │                        [*] Networking support  --->                                                                   │ │  
  │ │                            Device Drivers  --->                                                                       │ │  
  │ │                            Firmware Drivers  --->                                                                     │ │  
  │ │                            File systems  --->                                                                         │ │  
  │ │                        [ ] Virtualization  ----                                                                       │ │  
  │ │                            Kernel hacking  --->                                                                       │ │  
  │ │                            Security options  --->                                                                     │ │  
  │ │                        -*- Cryptographic API  --->                                                                    │ │  
  │ │                            Library routines  --->                                                                     │ │  
  │ │                                                                                                                       │ │  
  │ │                                                                                                                       │ │  
  │ │                                                                                                                       │ │  
  │ └───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘ │  
  ├───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┤  
  │                                 <Select>    < Exit >    < Help >    < Save >    < Load >                                  │  
  └───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘  
```

修改内核后缀

``` text
General setup  ---> 
		(-v8) Local version - append to kernel release
```

加入cgroup内核支持

``` text
General setup  ---> 
    -*- Control Group support  --->
        [*]   Memory controller          
	    [*]     Swap controller                               
        [*]       Swap controller enabled by default (NEW)     
        [*]   IO controller                        
        -*-   CPU controller  --->                  
            [*]     CPU bandwidth provisioning for FAIR_GROUP_SCHED    
            [*]   Group scheduling for SCHED_RR/FIFO                    
        [*]   PIDs controller                        
        [*]   RDMA controller                         
        [*]   Freezer controller                       
        [*]   Cpuset controller     
        [*]     Include legacy /proc/<pid>/cpuset file  
        [*]   Device controller      
        [*]   Simple CPU accounting controller                               

```

加入checkpoint支持

```
General setup  ---> 
    [*] Checkpoint/restore support  
```

加入KVM支持
```
[*] Virtualization  --->
    [*]   Kernel-based Virtual Machine (KVM) support
    <M>   Host kernel accelerator for virtio net
    [*]   Cross-endian support for vhost
```

勾选你需要的驱动

比如rtlusb网卡驱动

```
Device Drivers  --->
    [*] Network device support  --->
        [*]   Wireless LAN  --->
            [*]   Realtek devices                      
		    <M>   Realtek 8187 and 8187B USB support               
            <M>     Realtek rtlwifi family of devices  --->           
		    <M>     Realtek 8192C USB WiFi                             
            <M>    RTL8723AU/RTL8188[CR]U/RTL819[12]CU (mac80211) support       
```

选择完成后选择exit按钮退出并保存


## 编译内核

确保bc的安装

	$ sudo apt install bc

开始编译

	$ make -j5

* -jx 表示可以同时进行编译的进程数建议为cpu线程数+1  比如双核4线程就取5

# 制作镜像模板


创建工作文件夹

	$ mkdir ~/images/

进入工作文件夹

	$ cd ~/images/

创建一个300MB的镜像模板

	$ dd bs=1M count=300 if=/dev/zero of=300M_TEMPLATE.img


为镜像模板分区

	$ cfdisk 300M_TEMPLATE.img


``` text











                                       ┌ Select label type ───┐
                                       │ gpt                  │
                                       │ dos     *            │
                                       │ sgi                  │
                                       │ sun                  │
                                       └──────────────────────┘










                        Device does not contain a recognized partition table.
                Select a type to create a new label or press 'L' to load script file.


```

分区表类型选dos

``` text

                                       Disk: 300M_TEMPLATE.img
                            Size: 300 MiB, 314572800 bytes, 614400 sectors
                                  Label: dos, identifier: 0x7cbd547b

    Device           Boot             Start         End      Sectors       Size      Id Type
>>  Free space                         2048      614399       612352       299M                       


















                      [ * New  ]  [  Quit  ]  [  Help  ]  [  Write ]  [  Dump  ]


                                 Create new partition from free space

```

选 new 创建分区

``` text


                                       Disk: 300M_TEMPLATE.img
                            Size: 300 MiB, 314572800 bytes, 614400 sectors
                                  Label: dos, identifier: 0x7cbd547b

    Device           Boot             Start         End      Sectors       Size      Id Type
>>  Free space                         2048      614399       612352       299M                       


















 Partition size: 100M


                May be followed by M for MiB, G for GiB, T for TiB, or S for sectors.


```

分区大小为100M

``` text

                                       Disk: 300M_TEMPLATE.img
                            Size: 300 MiB, 314572800 bytes, 614400 sectors
                                  Label: dos, identifier: 0x7cbd547b

    Device           Boot             Start         End      Sectors       Size      Id Type
>>  Free space                         2048      614399       612352       299M                       


















                                        [*primary ]  [extended]


                                    0 primary, 0 extended, 4 free

```

分区类型选primary，主分区

``` text

                                       Disk: 300M_TEMPLATE.img
                            Size: 300 MiB, 314572800 bytes, 614400 sectors
                                  Label: dos, identifier: 0x7cbd547b

    Device                  Boot           Start        End     Sectors      Size     Id Type
>>  300M_TEMPLATE.img1      *               2048     206847      204800      100M     83 Linux        
    Free space                            206848     614399      407552      199M













 ┌──────────────────────────────────────────────────────────────────────────────────────────────────┐
 │Partition type: Linux (83)                                                                        │
 │    Attributes: 80                                                                                │
 └──────────────────────────────────────────────────────────────────────────────────────────────────┘
    [ * Bootable]  [ Delete ]  [ Resize ]  [  Quit  ]  [  Type  ]  [  Help  ]  [  Write ]  [  Dump  ]


```

选bootable使刚才创建的分区设定为可启动

``` text

                                       Disk: 300M_TEMPLATE.img
                            Size: 300 MiB, 314572800 bytes, 614400 sectors
                                  Label: dos, identifier: 0x7cbd547b

    Device                  Boot           Start        End     Sectors      Size     Id Type
>>  300M_TEMPLATE.img1      *               2048     206847      204800      100M     83 Linux        
    Free space                            206848     614399      407552      199M













 ┌──────────────────────────────────────────────────────────────────────────────────────────────────┐
 │Partition type: Linux (83)                                                                        │
 │    Attributes: 80                                                                                │
 └──────────────────────────────────────────────────────────────────────────────────────────────────┘
    [Bootable]  [ Delete ]  [ Resize ]  [  Quit  ]  [ * Type  ]  [  Help  ]  [  Write ]  [  Dump  ]


                                      Change the partition type


```

选type改变分区标识

``` text

                               ┌ Select partition type ──────────────┐
                               │  0 Empty                            │
                               │  1 FAT12                            │
                               │  2 XENIX root                       │
                               │  3 XENIX usr                        │
                               │  4 FAT16 <32M                       │
                               │  5 Extended                         │
                               │  6 FAT16                            │
                               │  7 HPFS/NTFS/exFAT                  │
                               │  8 AIX                              │
                               │  9 AIX bootable                     │
                               │  a OS/2 Boot Manager                │
                               │  b W95 FAT32          ****          │
                               │  c W95 FAT32 (LBA)                  │
                               │  e W95 FAT16 (LBA)                  │
                               │  f W95 Ext'd (LBA)                  │
                               │ 10 OPUS                             │
                               │ 11 Hidden FAT12                     │
                               │ 12 Compaq diagnostics               │
                               │ 14 Hidden FAT16 <32M                │
                               │ 16 Hidden FAT16                     │
                               │ 17 Hidden HPFS/NTFS                 │
                               │ 18 AST SmartSleep                   │
                               │ 1b Hidden W95 FAT32                 │
                               │ 1c Hidden W95 FAT32 (LBA)           │
                               └───────────────────────────────────↓─┘




```

选择找到W95 FAT32 并回车

``` text


                                       Disk: 300M_TEMPLATE.img
                            Size: 300 MiB, 314572800 bytes, 614400 sectors
                                  Label: dos, identifier: 0x7cbd547b

    Device                  Boot           Start        End     Sectors      Size     Id Type
    300M_TEMPLATE.img1      *               2048     206847      204800      100M      b W95 FAT32
>>  Free space                            206848     614399      407552      199M                     

















                      [ *  New  ]  [  Quit  ]  [  Help  ]  [  Write ]  [  Dump  ]


                                 Create new partition from free space


```

按方向健下，切换到空余空间.选择new新健一个分区

``` text

                                       Disk: 300M_TEMPLATE.img
                            Size: 300 MiB, 314572800 bytes, 614400 sectors
                                  Label: dos, identifier: 0x7cbd547b

    Device                  Boot           Start        End     Sectors      Size     Id Type
    300M_TEMPLATE.img1      *               2048     206847      204800      100M      b W95 FAT32
>>  Free space                            206848     614399      407552      199M                     

















 Partition size: 199M


                May be followed by M for MiB, G for GiB, T for TiB, or S for sectors.

```

分区使用所有空余空间

``` text

                                       Disk: 300M_TEMPLATE.img
                            Size: 300 MiB, 314572800 bytes, 614400 sectors
                                  Label: dos, identifier: 0x7cbd547b

    Device                  Boot           Start        End     Sectors      Size     Id Type
    300M_TEMPLATE.img1      *               2048     206847      204800      100M      b W95 FAT32
>>  Free space                            206848     614399      407552      199M                     

















                                        [ * primary]  [extended]


                                    1 primary, 0 extended, 3 free


```

选择primary 主分区

``` text

                                       Disk: 300M_TEMPLATE.img
                            Size: 300 MiB, 314572800 bytes, 614400 sectors
                                  Label: dos, identifier: 0x7cbd547b

    Device                  Boot           Start        End     Sectors      Size     Id Type
    300M_TEMPLATE.img1      *               2048     206847      204800      100M      b W95 FAT32
>>  300M_TEMPLATE.img2                    206848     614399      407552      199M     83 Linux        














 ┌──────────────────────────────────────────────────────────────────────────────────────────────────┐
 │Partition type: Linux (83)                                                                        │
 └──────────────────────────────────────────────────────────────────────────────────────────────────┘
    [Bootable]  [ Delete ]  [ Resize ]  [  Quit  ]  [  Type  ]  [  Help  ]  [ * Write ]  [  Dump  ]


                       Write partition table to disk (this might destroy data)


```

选择Write 将更改写入镜像

``` text

                                       Disk: 300M_TEMPLATE.img
                            Size: 300 MiB, 314572800 bytes, 614400 sectors
                                  Label: dos, identifier: 0x7cbd547b

    Device                  Boot           Start        End     Sectors      Size     Id Type
    300M_TEMPLATE.img1      *               2048     206847      204800      100M      b W95 FAT32
>>  300M_TEMPLATE.img2                    206848     614399      407552      199M     83 Linux        














 ┌──────────────────────────────────────────────────────────────────────────────────────────────────┐
 │Partition type: Linux (83)                                                                        │
 └──────────────────────────────────────────────────────────────────────────────────────────────────┘
 Are you sure you want to write the partition table to disk? yes


                        Type "yes" or "no", or press ESC to leave this dialog.

```

输入yes，回车

``` text


                                       Disk: 300M_TEMPLATE.img
                            Size: 300 MiB, 314572800 bytes, 614400 sectors
                                  Label: dos, identifier: 0x7cbd547b

    Device                  Boot           Start        End     Sectors      Size     Id Type
    300M_TEMPLATE.img1      *               2048     206847      204800      100M      b W95 FAT32
>>  300M_TEMPLATE.img2                    206848     614399      407552      199M     83 Linux        














 ┌──────────────────────────────────────────────────────────────────────────────────────────────────┐
 │Partition type: Linux (83)                                                                        │
 └──────────────────────────────────────────────────────────────────────────────────────────────────┘
    [Bootable]  [ Delete ]  [ Resize ]  [ *  Quit  ]  [  Type  ]  [  Help  ]  [  Write ]  [  Dump  ]


                                 Quit program without writing changes


```

选择quit退出


## 为镜像模板创建文件系统


使用kpartx挂载镜像到loop

	$ sudo kpartx -av 300M_TEMPLATE.img

输出的信息就是挂载到的目标设备文件

``` text
add map loop0p1 (254:0): 0 204800 linear 7:0 2048
add map loop0p2 (254:1): 0 407552 linear 7:0 206848
```

loop0p1 为第一个分区
loop0p2 为第二个分区

安装dosfstool

	$ sudo apt install dosfstools

格式化第一个分区为FAT32

	$ sudo mkfs.vfat /dev/mapper/loop0p1

格式化第二个分区为ext4

	$ sudo mkfs.ext4 /dev/mapper/loop0p2

如果你想把第二个分区格式化为f2fs

安装f2fs-tools

	$ sudo apt install f2fs-tools

格式化第二个分区为f2fs

	$ sudo mkfs.f2fs -f /dev/mapper/loop0p2

## 挂载镜像的boot分区

创建挂载点

	$ sudo mkdir /mnt/loop0p1

挂载

	$ sudo mount /dev/mapper/loop0p1 /mnt/loop0p1

## 复制内核和boot所需文件

复制所需文件
	
	$ cp boot/* /mnt/loop0p1/

进入编译的内核的工作目录

	$ cd ~/kernel/linux/

复制编译好的内核

	$ sudo cp ./arch/arm64/boot/Image /mnt/loop0p1/kernel8.img

将32位的dtb文件更名
	
	$ sudo mv /mnt/loop0p1/bcm2710-rpi-3-b.dtb /mnt/loop0p1/bcm2710-rpi-3-b.dtb_32

	$ sudo mv /mnt/loop0p1/bcm2710-rpi-3-b-plus.dtb /mnt/loop0p1/bcm2710-rpi-3-b-plus.dtb_32

复制64位的dtb文件

	$ sudo cp ./arch/arm64/boot/dts/broadcom/bcm2710-rpi-3-b.dtb /mnt/loop0p1/bcm2710-rpi-3-b.dtb

	$ sudo cp ./arch/arm64/boot/dts/broadcom/bcm2710-rpi-3-b-plus.dtb /mnt/loop0p1/bcm2710-rpi-3-b-plus.dtb

## 写cmdline.txt

cmdline.txt写有内核启动时的启动参数

	$ sudo vim /mnt/loop0p1/cmdline.txt

写入,并保存退出:

``` text
root=/dev/mmcblk0p2 rootfstype=ext4 rw rootwait fsck.repair=yes
```

* root=/dev/mmcblk0p2    将内存卡第二分区设置为根分区

* rootfstype=ext4        根分区类型 f2fs应更换为f2fs

* rw					 可写挂载跟分区

* rootwait				 等待内核识别根分区设备后再挂载

* fsck.repair=yes		 启动时自动检查修复文件系统错误

操作完成后的/mnt/loop0p1就像这样子

``` text
bcm2708-rpi-0-w.dtb     bcm2709-rpi-2-b.dtb       bcm2710-rpi-3-b-plus.dtb_32                fixup_db.dat      start_cd.elf
bcm2708-rpi-b.dtb       bcm2710-rpi-3-b.dtb       bcm2710-rpi-cm3.dtb          COPYING.linux  fixup_x.dat       start_db.elf
bcm2708-rpi-b-plus.dtb  bcm2710-rpi-3-b.dtb_32    bootcode.bin                 fixup_cd.dat   kernel8.img       start.elf
bcm2708-rpi-cm.dtb      bcm2710-rpi-3-b-plus.dtb  cmdline.txt                  fixup.dat      LICENCE.broadcom  start_x.elf
```

## 挂载根分区

创建挂载点

	$ sudo mkdir /mnt/loop0p2

挂载

	$ sudo mount /dev/mapper/loop0p2 /mnt/loop0p2

## 安装内核模块和固件

进入编译内核的工作目录

	$ cd ~/kernel/linux

切换root权限

	$ su

并把工具链环境变量应用

	# cd /home/你的用户名/toolchains/你的工具链目录/

	# source env.sh

把内核编译环境变量应用

	# cd /home/你的用户名/kernel/

	# source env.sh

进入内核编译的工作目录

	# cd /home/你的用户名/kernel/linux

安装模块

	# make modules_install INSTALL_MOD_PATH=/mnt/loop0p2


安装固件

创建固件文件夹

	# mkdir -p /mnt/loop0p2/lib/firmware/brcm

拷贝固件到目标文件夹

	# cp wifi固件 /mnt/loop0p2/lib/firmware/brcm/

	# cp 蓝牙固件 /mnt/loop0p2/lib/firmware/brcm/

退出root

	# exit


镜像模板创建完成

卸载挂载

	$ sync   # 同步写入
	$ sudo umount /dev/loop0p1
	$ sudo umount /dev/loop0p2
	$ sudo kpartx -dv /dev/loop0
	$ sudo losetup -d /dev/loop0

# 制作rootfs

创建工作文件夹

	$ mkdir ~/rootfs/

使用debootstrap制作rootfs

	$ sudo debootstrap --arch=arm64 发行代号 rootfs/发行代号


使用现成rootfs (使用root解压)

Archlinux 64 位官方rootfs:

http://mirrors.ustc.edu.cn/archlinuxarm/os/ArchLinuxARM-aarch64-latest.tar.gz

Gentoo 64 位官方rootfs:

http://distfiles.gentoo.org/experimental/arm64/

Ubuntu 64 位官方rootfs:

* xenial:

http://mirrors.ustc.edu.cn/ubuntu-cdimage/ubuntu-base/releases/xenial/release/ubuntu-base-16.04.5-base-arm64.tar.gz

* bionic:

http://mirrors.ustc.edu.cn/ubuntu-cdimage/ubuntu-base/releases/bionic/release/ubuntu-base-18.04.1-base-arm64.tar.gz

其他没有提供rootfs的发行可以通过像debootstarp一样的工具构建

你也可以自行使用CLFS构建一个rootfs

或者构建busybox


制作完成后 rootfs/发行代号 文件夹看起来大概是这样

``` text
ls rootfs/发行代号
bin
boot
dev
etc
home
lib
lib64
lost+found
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var

```

## 使用chroot进去安装配置

	$ cd rootfs/发行代号
	

挂载必要的文件系统

	$ sudo mount -o bind /dev/ dev
	$ sudo mount -o bind /dev/pts dev/pts
	$ sudo mount -o bind /proc proc
	$ sudo mount -o bind /sys sys

将外部的网络配置临时复制进rootfs

备份原来的配置

	$ sudo cp etc/hosts etc/hosts.bak
	$ sudo cp etc/resolv.conf etc/resolv.conf.bak

复制外部配置

	$ sudo cp /etc/hosts etc/hosts
	$ sudo cp /etc/resolv.conf etc/resolv.conf


chroot 进

	$ sudo chroot ./ /bin/sh

* chroot # 指代chroot内的操作

* 这里拿debian系的系统演示，其他系统请自行更换命令

进入后:

	chroot #

设置root密码

	chroot # passwd

添加新用户

	chroot # adduser user

为新用户设置密码

	chroot # passwd user

安装必要的软件

	chroot # apt update

	chroot # apt install vim wpasupplicant net-tools 

安装音频管理

	chroot # apt install alsa-utils

安装蓝牙管理

	chroot # apt install bluez

安装ssh

	chroot # apt install openssh-server


安装bash补全(可选)

	chroot # apt install bash-com*

安装桌面(可选)

	chroot # apt install ligthdm    # 登陆管理器

xfce4：

	chroot # apt install xfce4

lxde:

	chroot # apt install lxde

mate:

	chroot # apt install mate

细节不过多描述，用过gentoo的用户应该懂

* 不同系统需要修改配置的内容不同，请根据具体情况操作



### Slackware

再列举个sysv发行的初始化

去官方开发者源下载rootfs

ftp://ftp.arm.slackware.com/slackwarearm/slackwarearm-devtools/minirootfs/roots

解压并chroot进去后:

设置密码

添加用户

为用户设置密码

上面的操作和debian一样

设置开机项:


	chroot # cd /etc/rc.d/

启用开机项:

	chroot # chmod +x 服务脚本名

禁用开机项:

	chroot # chmod -x 服务脚本名

### 最小化busybox

建议拿lxc构建的出来自己改


## chroot 设置完成后

退出chroot

	chroot # exit

卸载文件系统

	$ sudo umount dev/pts
	$ sudo umount proc
	$ sudo umount sys

恢复网络配置

	$ sudo mv etc/hosts.bak etc/hosts
	$ sudo mv etc/resolv.conf.bak etc/resolv.conf

## 计算rootfs大小

	$ sudo du -sh ./
	
## 计算新镜像大小

    新镜像大小 = rootfs大小 + 原模板大小 + 200M

计算完成后记录下来

# 打包进镜像

进入镜像工作目录

	$ cd ~/images/

复制模板

	$ cp 300M_TEMPLATE 新镜像大小.img

扩展新镜像大小

	$ dd bs=1M count=rootfs大小（以m为单位) if=/dev/zero >> 新镜像大小.img

扩展新镜像第二分区

	$ cfdisk 新镜像大小.img

``` text

                                                         Disk: 1000M.img
                                        Size: 1000 MiB, 1048576000 bytes, 2048000 sectors
                                                Label: dos, identifier: 0x7cbd547b

    Device              Boot                   Start             End         Sectors          Size         Id Type
    1000M.img1          *                       2048          206847          204800          100M          b W95 FAT32
>>  1000M.img2                                206848          614399          407552          199M         83 Linux               
    Free space                                614400         2047999         1433600          700M



















 ┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ Partition type: Linux (83)                                                                                                   │
 │Filesystem UUID: c34348cf-fbb3-4056-a45c-70e28309bfb5                                                                         │
 │     Filesystem: f2fs                                                                                                         │
 └──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
                  [Bootable]  [ Delete ]  [  * Resize ]  [  Quit  ]  [  Type  ]  [  Help  ]  [  Write ]  [  Dump  ]


                                               Quit program without writing changes

```

使用方向键下 移动到第二分区 选择resize

``` text

                                                         Disk: 1000M.img
                                        Size: 1000 MiB, 1048576000 bytes, 2048000 sectors
                                                Label: dos, identifier: 0x7cbd547b

    Device              Boot                   Start             End         Sectors          Size         Id Type
    1000M.img1          *                       2048          206847          204800          100M          b W95 FAT32
>>  1000M.img2                                206848          614399          407552          199M         83 Linux               
    Free space                                614400         2047999         1433600          700M



















 ┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ Partition type: Linux (83)                                                                                                   │
 │Filesystem UUID: c34348cf-fbb3-4056-a45c-70e28309bfb5                                                                         │
 │     Filesystem: f2fs                                                                                                         │
 └──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
 New size: 899M


                              May be followed by M for MiB, G for GiB, T for TiB, or S for sectors.


```

输入最大的分区大小  （第二分区 + 空闲区域)

``` text

                                                         Disk: 1000M.img
                                        Size: 1000 MiB, 1048576000 bytes, 2048000 sectors
                                                Label: dos, identifier: 0x7cbd547b

    Device              Boot                   Start             End         Sectors          Size         Id Type
    1000M.img1          *                       2048          206847          204800          100M          b W95 FAT32
>>  1000M.img2                                206848         2047999         1841152          899M         83 Linux               




















 ┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ Partition type: Linux (83)                                                                                                   │
 │Filesystem UUID: c34348cf-fbb3-4056-a45c-70e28309bfb5                                                                         │
 │     Filesystem: f2fs                                                                                                         │
 └──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
                  [Bootable]  [ Delete ]  [ Resize ]  [  Quit  ]  [  Type  ]  [  Help  ]  [ *  Write ]  [  Dump  ]


                                     Write partition table to disk (this might destroy data)

```

选择Write写入

``` text

                                                         Disk: 1000M.img
                                        Size: 1000 MiB, 1048576000 bytes, 2048000 sectors
                                                Label: dos, identifier: 0x7cbd547b

    Device              Boot                   Start             End         Sectors          Size         Id Type
    1000M.img1          *                       2048          206847          204800          100M          b W95 FAT32
>>  1000M.img2                                206848         2047999         1841152          899M         83 Linux               




















 ┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ Partition type: Linux (83)                                                                                                   │
 │Filesystem UUID: c34348cf-fbb3-4056-a45c-70e28309bfb5                                                                         │
 │     Filesystem: f2fs                                                                                                         │
 └──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
 Are you sure you want to write the partition table to disk? yes


                                      Type "yes" or "no", or press ESC to leave this dialog.

```

yes同意

``` text

                                                         Disk: 1000M.img
                                        Size: 1000 MiB, 1048576000 bytes, 2048000 sectors
                                                Label: dos, identifier: 0x7cbd547b

    Device              Boot                   Start             End         Sectors          Size         Id Type
    1000M.img1          *                       2048          206847          204800          100M          b W95 FAT32
>>  1000M.img2                                206848         2047999         1841152          899M         83 Linux               




















 ┌──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ Partition type: Linux (83)                                                                                                   │
 │Filesystem UUID: c34348cf-fbb3-4056-a45c-70e28309bfb5                                                                         │
 │     Filesystem: f2fs                                                                                                         │
 └──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘
                  [Bootable]  [ Delete ]  [ Resize ]  [   * Quit  ]  [  Type  ]  [  Help  ]  [  Write ]  [  Dump  ]


                                               Quit program without writing changes

```

选quit退出

## 扩展分区

loop挂载

	$ sudo kpartx -av 新镜像大小.img

使用工具扩展分区

ext4:

	$ sudo resize2fs /dev/mapper/loop0p2

f2fs:

	$ sudo resize.f2fs /dev/mapper/loop0p2


不要卸载，下一步同步rootfs进镜像会用到第二分区

# 打包进镜像

## 同步rootfs进镜像第二分区

进入rootfs目录

	$ cd ~/rootfs/发行代号/

挂载第二分区

	$ sudo mount /dev/mapper/loop0p2 /mnt/loop0p2

同步

安装rsync

	$ sudo apt install rsync

同步进去

	$ sudo rsync -HPavz -q ./ /mnt/loop0p2

等待同步完成

卸载

	$ sync
	$ sudo umount /mnt/loop0p2
	$ sudo kpartx -dv /dev/loop0
	$ sudo losetup -d /dev/loop0

得到一个新镜像

# 测试


自行刷入测试



# 一些问题

* 镜像刷入后黑屏无法启动

检查内核编译环节和文件cmdline.txt

* 内核启动后panic

检查rootfs和内核编译环节

* 启动后wifi没有

检查固件和内核编译环节

* f2fs不能在运行中扩展分区

拆卡到电脑上扩展

-------------------------

luhux@hotmail.com

请勿商业转载

转载请标明出处

------------------------
