---
title: LaTex 中的常用宏包
description: LaTex 中的常用宏包，提供了额外的数学公式选项和数学字体，可以解决报错Undefined control sequence.(\url)，并提供链接跳转等功能。
date: 2024/03/20 22:48:01
updated: 2024/03/21 15:31:54
categories:
  - Tool
  - LaTex
tags:
  - LaTex
---

## 数学公式相关

```tex
\usepackage{amsmath}  % 提供了额外的数学公式选项
\usepackage{amsfonts} % 提供了额外的数学字体，比如 \mathbb
\usepackage{amssymb}  % 提供了更多的数学符号

\usepackage{anyfontsize} % 某些字体在特定大小下不可用，可能会引发警告信息，这个可以抑制警告
```

## BibTex编译相关

```tex
\usepackage{hyperref} % 解决报错Undefined control sequence.(\url)，并提供链接跳转功能
```
