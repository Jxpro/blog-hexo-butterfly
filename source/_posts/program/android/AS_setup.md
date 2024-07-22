---
title: Android Studio 安装与使用
description: Android Studio 安装与配置，修改 .gradle 和 AVD 文件位置，遇到的问题及解决方法。
date: 2022/05/01 16:35:00
updated: 2022/05/03 16:51:56
categories:
  - Program
  - Android
tags:
  - Program
  - Android
---

## 安装与配置

1.   官网下载[Android Studio](https://developer.android.com/studio)并进行安装，然后打开

2.   提示第一次启动无法访问`Android SDK`，直接`cancel`即可，后面安装会再次下载

     ![image-20220501161242605](https://raw.githubusercontent.com/Jxpro/PicBed/master/md/new/2022-05-01-161244.png)

3.   进入欢迎页面，直接下一步

4.   安装类型，选择自定义，以修改文件存储目录

5.   然后选择环境UI，SDK的安装位置

6.   然后是模拟器相关设置，通常默认即可

7.   最后确认安装，等待下载完成

## 修改.gradle位置

### 问题

使用`Android Studio`时，默认在C盘用户目录下创建`.gradle`文件夹存放`Gradle`相关文件，一般占用空间较大，但如果在当前项目设置里的`gradle`文件夹路径，每次新建项目后都会重置为C盘下的路径。

下面提供两种解决方案，但是特别注意的是，**自定义的.gradle路径必须要让所有用户都具有读写权限！！！**

### 方法一

>   该方法才是**最终解决方案**，直接修改系统默认值，最有效！！！
>
>   设置了这个就可以不用设置下面的方法二了。

在系统环境变量中添加`GRADLE_USER_HOME`，值为自定义的`.gradle`文件夹路径，修改后最好重启。

![image-20220501162655053](https://raw.githubusercontent.com/Jxpro/PicBed/master/md/new/2022-05-01-162656.png)

### 方法二

>   该方法**有bug**，只有新建项目的名字是用过一次的名字才有效！！！

1.   首先关闭当前项目

     ![image-20220501162920046](https://raw.githubusercontent.com/Jxpro/PicBed/master/md/new/2022-05-01-162922.png)

2.   然后在全局配置里修改gradle路径

     ![image-20220501162947823](https://raw.githubusercontent.com/Jxpro/PicBed/master/md/new/2022-05-01-162950.png)

     ![image-20220501163013655](https://raw.githubusercontent.com/Jxpro/PicBed/master/md/new/2022-05-01-163017.png)

## 修改AVD文件位置

默认的AVD是下载在`C:\Users\用户名\.android\avd`文件夹下的，一个`AVD`往往占用4G以上的内存空间。

为了释放C盘空间，可以将上述文件夹中的`除ini文件以外`的文件剪贴到电脑的任意位置。

再逐个将`C:\Users\用户名\.android\avd`文件夹里面的`ini文件`，将`path`修改为你自己`avd`存放的路径即可。

>   -   每一个`ini`文件对应一个`avd`文件
>   -   路径中尽量避免空格

## HAXM安装失败

### 问题

>   Intel® HAXM installation failed.
>
>   To install Intel® HAXM follow the instructions found at: https://github.com/intel/haxm/wiki/Installation-Instructions-on-Windows

### 方法

这大概率是由于`Hyper-V`未彻底关闭造成的，解决办法有两个：

-   一是使用官网提供工具：[dgreadiness_v3.6](https://www.microsoft.com/en-us/download/details.aspx?id=53337)，下载完后解压，然后在 `Powershell` 中运行

    ```
    DG_Readiness_Tool_v3.6.ps1 -Disable
    ```

-   二是使用知乎大佬提供的脚本，将下面代码保存为bat文件并以管理员运行：

    ```
    @echo off

    dism /Online /Disable-Feature:microsoft-hyper-v-all /NoRestart
    dism /Online /Disable-Feature:IsolatedUserMode /NoRestart
    dism /Online /Disable-Feature:Microsoft-Hyper-V-Hypervisor /NoRestart
    dism /Online /Disable-Feature:Microsoft-Hyper-V-Online /NoRestart
    dism /Online /Disable-Feature:HypervisorPlatform /NoRestart


    REM ===========================================

    mountvol X: /s
    copy %WINDIR%\System32\SecConfig.efi X:\EFI\Microsoft\Boot\SecConfig.efi /Y
    bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
    bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
    bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
    bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
    bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=X:
    mountvol X: /d
    bcdedit /set hypervisorlaunchtype off
    REG DELETE HKLM\SYSTEM\CurrentControlSet\Control\DeviceGuard /v EnableVirtualizationBasedSecurity /f

    echo.
    echo.
    echo.
    echo.
    echo 接下来 请重新启动您的计算机，完成剩下的操作。
    echo 请注意！重启时的屏幕提示！
    echo PS：在重启时，过了BIOS自检之后，看到黑白字符提示你按键的时候……
    echo ……死按，狂按 F3键，只到它自动重启为止！!
    echo 可以关闭此窗口了，重启电脑吧。。。
    pause > nul
    echo.
    echo.
    ```

上述方法二选一，运行完以后都需要重启，重启时如有提示，请根据提示进行操作

## Android SDK Manager 无法启动

可能的问题：

1. `SDK/tools`的问题
2. 排查`find_java.bat`文件
3. 删除 `C:\Windows\system32\`下的`java.exe、javaw.exe、javaws.exe`

### 情况 1

参考：[android SDK SDK Manager.exe 无法打开，一闪而过最终解决办法](https://blog.csdn.net/wang295689649/article/details/60960953)

方法：到[AndroidDevTools](https://www.androiddevtools.cn/)下载下列压缩包，替换掉`SDK/tools`下的内容即可

![image-20220501160022209](https://raw.githubusercontent.com/Jxpro/PicBed/master/md/new/2022-05-01-160023.png)

### 情况2

参考：

[android sdk manager无法启动，一闪而过的解决方案](https://blog.csdn.net/yubin_yubin/article/details/8916389)

[Android SDK总结 SDK Manager无法启动、一闪而过的解决办法](https://blog.csdn.net/hueise_h/article/details/9134237)

方法：到`SDK\tools\lib`下，`cmd`运行`find_java.bat`，看到如下输出：

![image-20220501160300114](https://raw.githubusercontent.com/Jxpro/PicBed/master/md/new/2022-05-01-160301.png)

说明**jdk版本（或发行商）**存在问题，修改`java_home`的指向，再次运行，输出未空则说明没有问题：

![image-20220501160339195](https://raw.githubusercontent.com/Jxpro/PicBed/master/md/new/2022-05-01-160340.png)
