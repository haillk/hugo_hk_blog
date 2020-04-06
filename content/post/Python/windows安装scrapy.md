---
title: "Windows通过pip安装scrapy"
date: 2019-12-10T18:29:22+08:00
categories:
- Python
tags:
- Scrapy
keywords:
- Scrapy
#thumbnailImage: //example.com/image.jpg
---

Windows通过pip安装scrapy



Scrapy需要超多的依赖，而`pip instarll scrapy`并不会下载好全部依赖。我在安装twisted时出现问题。

```shell
error: Microsoft Visual C++ 14.0 is required. Get it with "Microsoft Visual C++ Build Tools": https://visualstudio.microsoft.com/downloads/
```

这是因为twisted安装依赖pywin32和visual build tools.

#### 安装build tools

下载Visual C++ Build Tools  [Visual C++ Build Tools 下载链接](https://go.microsoft.com/fwlink/?LinkId=691126)

选择默认配置，安装即刻。

#### 安装pywin32

emmmm 不过呢，现在是2019/12/10 ,目前还没有python3.8版本的pywin32。

所以说啊，还是要用老版本的python,默默安装python3.7....................................................

[ python3.7之前的pywin32 下载链接](https://sourceforge.net/projects/pywin32/files/pywin32/Build%20221/)

选择好了对应版本之后，下载下来一直下一步就可以了。

#### 安装scrapy

最后安装scrapy就好了。`pip install scrapy`

#### 创建新项目时出现问题：cannot import name 'etree' from 'lxml'

emmmm卸载原来的再安装lxml

```shell
pip uninstall lxml
pip install lxml
```

