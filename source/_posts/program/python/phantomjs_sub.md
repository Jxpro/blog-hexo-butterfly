---
title: PhantomJS 爬虫工具替代方案
description: PhantomJS 是一个基于 WebKit 的服务器端 JavaScript API，用于无界面浏览器的访问，目前已经停止开发，推荐使用 Chrome 或 Firefox 的 headless 版本替代。
date: 2021/12/17 20:11:00
updated: 2021/12/17 20:11:00
categories:
  - Program
  - Python
tags:
  - Program
  - Python
  - Crawler
---

## 介绍

在selenium@3.14.1中使用PhantomJS时候，出现提示：

**UserWarning: Selenium support for PhantomJS has been deprecated, please use headless versions of Chrome or Firefox instead**

意思是说：新版本的Selenium不再支持PhantomJS了，请使用Chrome或Firefox的headless版本替代。

查询谷歌，发现首先是PhantomJS开发者内部矛盾，并且Firefox和Chrome的headless模式带来的打压，最终宣布PhantomJS终止开发

## Headless Chrome 的使用

Headless模式在Windows中是**Chrome 59**中的新特征，要使用Chrome需要安装`chromedriver`。

```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options

chrome_options = Options()
chrome_options.add_argument('--headless')
driver = webdriver.Chrome(chrome_options=chrome_options)
driver.get("https://cnblogs.com/")
```

其他用法与PhantomJS基本相同，更多资料请查看官方文档。

参考资料：https://developers.google.com/web/updates/2017/04/headless-chrome
