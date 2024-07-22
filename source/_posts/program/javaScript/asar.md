---
title: Asar 文件介绍和解析
description: Asar 文件是一个归档文件，用于使用 Electron 为应用程序打包源代码。它以类似于.TAR 存档的格式保存，其将所有文件组合在一起，无需压缩，同时具有随机访问支持。
date: 2021/12/30 12:58:06
updated: 2022/01/28 20:05:32
categories:
  - Program
  - JavaScript
tags:
  - Program
  - Electron
  - JavaScript
---

## 认识Asar文件

>   .ASAR 文件是一个**归档文件**，用于使用Electron（一个用于构建跨平台程序的开放源代码库）为应用程序打包源代码。它以类似于.TAR 存档的格式保存，其将所有文件组合在一起，无需压缩，同时具有随机访问支持。

## 使用Asar文件

[安装nodejs和npm](http://nodejs.cn/download/)

>   npm是nodejs的包管理工具，我们后续需要通过npm安装asar，因此需要先安装npm
>
>   如果你的电脑之前已经安装了nodejs最新版，那么nodejs也就为你自动安装了npm

全局环境安装asar

```shell
npm install asar -g

asar -V
```

常用命令

-   打包一个文件或目录:

    ```shell
    asar pack ./ app.asar
    ```

-   解压一个 asar 文件:

    ```shell
    asar extract app.asar ./
    ```

-   列出一个 asar 文件中的内容:

    ```shell
    asar list app.asar
    ```

-   从 asar 文件中解压指定的文件:

    ```shell
    asar extract-file {{asar 文件}} {{文件}}
    ```
