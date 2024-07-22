---
title: PEP 8 - do not use bare 'exept'
description: PyCharm 警告，PEP 8 - do not use bare 'exept'。
date: 2021/12/16 22:31:35
updated: 2022/03/26 13:02:33
categories:
  - Program
  - Python
tags:
  - Program
  - Python
---

## 问题描述

![image-20211112222512429](https://raw.githubusercontent.com/Jxpro/PicBed/master/md/2021/11/12-222513.png)

## 原因一：[Flake8 Rules](https://www.flake8rules.com/)

>   Do not use bare except, specify exception instead (E722)

解决方法：`Inspection`的`option`里添加`E722`项

![image-20211112222613037](https://raw.githubusercontent.com/Jxpro/PicBed/master/md/2021/11/12-222614.png)

## 原因二：Unclear exception clauses

解决方法：取消勾选`Unclear exception clauses`

![image-20211112222736678](https://raw.githubusercontent.com/Jxpro/PicBed/master/md/2021/11/12-222738.png)
