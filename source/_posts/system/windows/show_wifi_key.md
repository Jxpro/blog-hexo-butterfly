---
title: Windows 查看电脑连接的 WiFi 密码
description: 查看 Windows 电脑连接过的 WiFi 名称和密码。
date: 2022/01/25 12:46:23
updated: 2022/01/25 12:46:23
categories:
  - System
  - Windows
tags:
  - System
  - Windows
---

打开`CMD`（`Terminal`不行），输入以下指令，查看电脑连接过的WiFi名称

```shell
netsh wlan show profiles
```

![list](https://raw.githubusercontent.com/Jxpro/PicBed/master/md/2021/11/17-220534.png)

输入以下指令，查看该WiFi名称的详细信息，如下图所示：

```shell
netsh wlan show profiles WiFi名称 key=clear
```

![cont](https://raw.githubusercontent.com/Jxpro/PicBed/master/md/2021/11/17-220817.png)

提示：如果WiFi名称为汉字不能输入，可以在其它地方输入后复制粘贴。
