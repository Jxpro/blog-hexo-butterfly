---
title: Windows 下 mklink 用法
description: Windows 下 mklink 可以创建目录符号链接、硬链接、目录联接。
date: 2022/01/26 17:23:14
updated: 2022/01/26 17:23:14
categories:
  - System
  - Windows
tags:
  - System
  - Windows
---

```shell
MKLINK [[/D] | [/H] | [/J]] Link Target

        /D      创建目录符号链接。默认为文件
                符号链接。
        /H      创建硬链接而非符号链接。
        /J      创建目录联接。
        Link    指定新的符号链接名称。
        Target  指定新链接引用的路径
                (相对或绝对)。
```
