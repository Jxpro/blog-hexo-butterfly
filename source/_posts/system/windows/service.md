---
title: Windows服务进程配置文件
description: Windows 下服务进程的配置文件会采用不同的路径。
date: 2024/05/17 18:40:28
updated: 2024/05/17 18:40:28
categories:
  - System
  - Windows
tags:
  - System
  - Windows
---

以`RustDesk`为例，正常前台运行的程序配置文件位于：

`C:\Users\username\AppData\Roaming\RustDesk\config\RustDesk.toml`

但是当其注册为服务进程，在后台运行时，其使用的配置文件位于，且会覆盖上面的文件：

`C:\Windows\ServiceProfiles\LocalService\AppData\Roaming\RustDesk\config\RustDesk.toml`
