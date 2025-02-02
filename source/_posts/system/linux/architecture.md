---
title: 如何查看 Linux 系统的架构
description: 查看 Linux 系统架构的5个命令，以及查看虚拟化方式。
date: 2021/10/13 11:54:55
updated: 2024/07/05 09:42:53
categories:
  - System
  - Linux
tags:
  - System
  - Linux
---

## Linux系统架构

linux的**系统架构**的表示有：`AMD`、`ARM`、`x86`、`x86_64`、`pcc`

## 查看内核版本

-   `cat /proc/version`
-   `uname -a`
-   `uname -r`

```shell
$ cat /proc/version
Linux version 4.15.0-161-generic (buildd@lcy01-amd64-012) (gcc version 7.5.0 (Ubuntu 7.5.0-3ubuntu1~18.04)) #169-Ubuntu SMP Fri Oct 15 13:41:54 UTC 2021

$ uname -a
Linux iZwz91577x7sn2pgjejf5kZ 4.15.0-161-generic #169-Ubuntu SMP Fri Oct 15 13:41:54 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux

$ uname -r
4.15.0-161-generic
```

## 查看linux版本信息

-   `lsb_release -a`
-   `cat /etc/issue`

```shell
$ lsb_release -a
LSB Version:	core-9.20170808ubuntu1-noarch:security-9.20170808ubuntu1-noarch
Distributor ID:	Ubuntu
Description:	Ubuntu 18.04.6 LTS
Release:	18.04
Codename:	bionic

$ cat /etc/issue
Ubuntu 18.04.6 LTS \n \l
```

## 查看linux是64位还是32位

-   `getconf LONG_BIT`
-   `file /bin/ls`

```shell
$ getconf LONG_BIT
64

$ file /bin/ls
/bin/ls: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 3.2.0, BuildID[sha1]=9567f9a28e66f4d7ec4baf31cfbf68d0410f0ae6, stripped
```

## 直接查看系统的架构

-   `dpkg --print-architecture`
-   `arch`
-   `file /lib/systemd/systemd`

```shell
$ dpkg --print-architecture
amd64

$ arch
x86_64

$ file /lib/systemd/systemd
/lib/systemd/systemd: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 3.2.0, BuildID[sha1]=cb01338bc2dce40f2dec56c8a2c89a7dac28d512, stripped
```

[关于CPU架构是x86、x86_64、amd64的叫法区别](https://blog.csdn.net/wf19930209/article/details/79536506)

## 查看Mint系统对应的Ubuntu系统

[参考](https://zhuanlan.zhihu.com/p/83166352)

使用`cat /etc/os-release`命令也可以获取到 Ubuntu 代号

```shell
$ cat /etc/os-release

# 输出
NAME="Ubuntu"
VERSION="18.04.6 LTS (Bionic Beaver)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 18.04.6 LTS"
VERSION_ID="18.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=bionic
UBUNTU_CODENAME=bionic
```

注意：

>   x86_64，x64，AMD64基本上是同一个东西

-   `x86`是`intel`开发的一种32位指令集
-   `x84_64`是CPU迈向64位的时候
-   x86_64是一种64位的指令集，x86_64是x86指令的超集，在x86上可以运行的程序，在x86_64上也可以运行，`x86_64`是AMD发明的，也叫`AMD64`

现在用的intel/amd的桌面级CPU基本上都是x86_64，与之相对的`arm`、`pcc`等都不是`x86_64`

## 查看虚拟化方式

-   `lscpu`
-   `systemd-detect-virt`

```shell
$ lscpu

Architecture:            x86_64
  CPU op-mode(s):        32-bit, 64-bit
  Address sizes:         46 bits physical, 48 bits virtual
  Byte Order:            Little Endian
CPU(s):                  2
  On-line CPU(s) list:   0,1
Vendor ID:               GenuineIntel
...
Virtualization features:
  Hypervisor vendor:     KVM
  Virtualization type:   full
Caches (sum of all):
...

$ systemd-detect-virt
kvm
```
