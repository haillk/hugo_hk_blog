---
title: 'Go代理和GO111MODULE设置'
tags: 
-
categories: 
- GoLang
date: "2020-07-18T19:43:35+08:00"
comment:  true    
---

与你常在

<!--more-->

### 设置代理

```shell
#这是中国可靠的Go模块代理
go env -w GOPROXY=https://goproxy.cn,direct
#
go env -w GO111MODULE=on
```

> GO111MODULE=off 无模块支持，go 会从 GOPATH 和 vendor 文件夹寻找包。 
>
> GO111MODULE=on 模块支持，go 会忽略 GOPATH 和 vendor 文件夹，只根据 go.mod 下载依赖。 GO111MODULE=auto 在 $GOPATH/src 外面且根目录有 go.mod 文件时，开启模块支持