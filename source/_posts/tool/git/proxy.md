---
title: Git 设置代理加速访问
description: 为 git 设置全局代理，科学上网以便于访问外网资源，解决 git clone 速度慢的问题。
date: 2021/10/02 16:12:43
updated: 2022/1/28 20:05:32
categories:
  - Tool
  - Git
tags:
  - Git
---

```shell
# 为 git 设置全局代理
git config --global https.proxy http://127.0.0.1:7890
git config --global https.proxy https://127.0.0.1:7890

# 取消设置代理
git config --global --unset http.proxy
git config --global --unset https.proxy

# 查看设置代理
git config --global --get http.proxy
git config --global --get https.proxy
```
