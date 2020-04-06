---
title: "使用requests"
date: 2019-12-25T17:46:22+08:00
lastmod: 2019-12-25T17:46:22+08:00
show_in_homepage: true

tags: []
categories: []

featured_image: ''
featured_image_preview: ''

comment: true
toc: false
autoCollapseToc: true
---

<!--more-->

## Can't connect to HTTPS URL because the SSL module is not available.在anaconda环境中使用requests.get()时提示的错误

解决方法：将正在使用的环境中的此目录下的libcrypto\*.\*  和libssl\*.\* 四个文件复制到该环境中的根目录下去。

from

```
D:\soft\Anaconda\envs\hk-learn\Library\bin
```

to

```
D:\soft\Anaconda\envs\hk-learn\
```

hk-learn是我的环境名。