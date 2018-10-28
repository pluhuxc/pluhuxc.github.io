---
layout: post
title: 编译安装aircrack-ng
tags: [aircrack-ng, wifi, linux]
author: Luhux
comment: true
---

# 安装aircrack-ng

aircrack-ng:
[下载](https://github.com/aircrack-ng/aircrack-ng/releases)

下载:
    
    $ wget https://github.com/aircrack-ng/aircrack-ng/archive/1.4.tar.gz

解压:

    $ tar zxf 1.4.tar.gz

进入:

    $ cd aircrack-ng-1.4

查看依赖;

* README.md

```
 28 ## Requirements
 29 
 30  * Autoconf
 31  * Automake
 32  * Libtool
 33  * shtool
 34  * OpenSSL development package or libgcrypt development package.
 35  * Airmon-ng (Linux) requires ethtool.
 36  * On windows, cygwin has to be used and it also requires w32api package.
 37  * On Windows, if using clang, libiconv and libiconv-devel
 38  * Linux: LibNetlink 1 or 3. It can be disabled by passing --disable-libnl to configure.
 39  * pkg-config (pkgconf on FreeBSD)
 40  * FreeBSD, OpenBSD, NetBSD, Solaris and OS X with macports: gmake
 41  * Linux/Cygwin: make and Standard C++ Library development package (Debian: libstdc++-dev)

 43 ## Optional stuff
 44 
 45  * If you want SSID filtering with regular expression in airodump-ng
 46    (-essid-regex) pcre development package is required.
 47  * If you want to use airolib-ng and '-r' option in aircrack-ng,
 48    SQLite development package >= 3.3.17 (3.6.X version or better is recommended)
 49  * If you want to use Airpcap, the 'developer' directory from the CD/ISO/SDK is required.
 50  * In order to build `besside-ng`, `besside-ng-crawler`, `easside-ng`, `tkiptun-ng` and `wesside-ng`,
 51    libpcap development package is required (on Cygwin, use the Aircap SDK instead; see above)
 52  * For best performance on FreeBSD (50-70% more), install gcc5 (or better) via: pkg install gcc7
 53  * rfkill
 54  * For best performance on SMP machines, ensure the hwloc library and headers are installed. It is strongly recommended on high core count systems, it may give a serious speed boost
 55  * CMocka for unit testing

```

 编译一个完整的aircrack-ng需要

* autoconf
* automake
* libtool
* shtool
* libssl-dev
* ethtool
* libnl-3-dev
* sqlite3
* libpcap-dev
* rfkill
* gcc
* make 
* pkg-config


## 在debian系安装依赖

    # apt install autoconf automake libtool shtool libssl1.0-dev ethtool libnl-3-dev sqlite3 libpcap-dev rfkill gcc make build-essential libnl-genl-3-dev zlib1g-dev libsqlite3-dev libpcre3-dev libcmocka-dev pkg-config 

## 其他系统请自行编译库并指定include路径

./autogen.sh

    $ ./autogen.sh

./configure 

    $ ./configure --enable-libnl CFLAGS="-O2"

make 
    
    $ make

安装:

    $ sudo make install 

