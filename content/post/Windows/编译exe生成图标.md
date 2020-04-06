---
title: "编译exe生成图标"
date: 2019-12-26T01:22:25+08:00
lastmod: 2019-12-26T01:22:25+08:00
show_in_homepage: true

tags: ['exe','图标']
categories: ['Windows']

featured_image: ''
featured_image_preview: ''

comment: true
toc: false
autoCollapseToc: true  
---

使用MinGW生成带有定制图标的exe

<!--more-->

> MinGW是什么？
>
> **MinGW**（**Min**imalist **G**NU for **W**indows），又称**mingw32**，是将[GCC](https://zh.wikipedia.org/wiki/GCC)编译器和[GNU Binutils](https://zh.wikipedia.org/wiki/GNU_Binutils)移植到Win32平台下的产物，包括一系列头文件（[Win32API](https://zh.wikipedia.org/wiki/Windows_API)）、[库](https://zh.wikipedia.org/wiki/靜態連結函式庫)和[可执行文件](https://zh.wikipedia.org/wiki/可执行文件)。
>
> 目前也有64位的支持。

## 生成exe

下面开始生成exe

- 在main.go  同级目录下新建文件main.rc 文件内容：

``` 
IDI_ICON1 ICON "ICON.ico"
```

- 还是在同级目录下放置你的ICON.ico图标问价
- 打开cmd输入

```
windres -o main.syso main.rc
```

- 之后会生成一个main.syso文件

- 最后go build就可以了

  ```
  go build -ldflags="-H windowsgui -w -s"
  ```

  notes:代码如果设计目录，那么目录中的目录分隔符要用"\\\\"

