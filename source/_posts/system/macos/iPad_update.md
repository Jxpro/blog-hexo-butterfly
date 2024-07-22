---
title: iTunes 跳过备份升级 iPad
description: iTunes 升级 iPad 时，跳过备份直接更新，以及 4000 错误的解决办法。
date: 2022/01/16 22:11:35
updated: 2022/01/28 20:05:32
categories:
  - System
  - MacOS
tags:
  - System
  - MacOS
---

## 跳过备份直接更新

### Windows

确保**iTunes已关闭**，进入`terminal`，输入以下指令

```shell
"D:\Program Files\iTunes\defaults.exe" write com.apple.iTunes AutomaticDeviceBackupsDisabled -bool true
# 或
"%CommonProgramFiles%\Apple\Apple Application Support\defaults.exe" write com.apple.iTunes AutomaticDeviceBackupsDisabled -bool true
```

### MacOS

确保**iTunes已关闭**，打开`terminal`，输入以下指令

```shell
defaults write com.apple.iTunes AutomaticDeviceBackupsDisabled -bool true
```

## 4000错误的解决办法

-   只需暂时关闭**屏幕密码**就可以了！！！
