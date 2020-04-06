---
title: "Selenium安装使用"
date: 2019-12-15T00:49:02+08:00
categories:
- Python
tags:
- selecnium
keywords:
- selenium安装与使用
#thumbnailImage: //example.com/image.jpg
---



selenium能真是的模拟打开一个浏览器，模拟鼠标事件，提供选择器，控制cookie等等。

#### <font color=#CC3300>安装</font>

由于使用了conda,安装就很方便了。

`conda install selenium`

#### <font color=#CC3300>安装chrome驱动</font>

[chrome驱动地址](https://chromedriver.storage.googleapis.com/index.html)

选择对应版本的驱动，解压得到chromedriver.exe,放到anaconda的python路径中去

`D:\soft\Anaconda\envs\hk-learn\Scripts`

#### <font color=#CC3300>验证驱动是否能正常使用</font>

```python
from selenium import webdriver
driver = webdriver.Chrome() 
```

能自动打开浏览器即可。