---
title: 'GoModule模块的使用'
tags: 
-
categories: 
- GoLang
date: "2020-07-12T19:52:40+08:00"
comment:  true    
---

与你常在

<!--more-->

[参考](https://learnku.com/go/t/38809)

### 创建模块

```powershell
#首先在src路径下创建一个模块的文件夹，编写模块内容
#之后使用gomod初始化
$go mod init github.com/haillk/testmod
go: creating new go.mod: module github.com/haillk/testmod
```

这会在该包目录西安创建一个名为go.mod的文件，内容如下

```go
module github.com/haillk/testmod
```

之后将该包上传到github即可。这就使我们的包变成了一个模块。

##### 上传和tag操作

```shell
git init
git add *
git commit -am "first commit"
git push -u origin master

git tag v1.0.0
git push --tags

git checkout -b v1
git push -u origin v1
```

### 使用模块

在文件中引用刚刚上传的包

```go
...
import "github.com/haillk/testmod"
...
```

使用时也要先初始化

```shell
go mod init test2
```

该步会创建go.mod文件

之后使用build命令构建

```shell
go build
go: finding github.com/haillk/testmod v1.0.0
go: downloading github.com/haillk/testmod v1.0.0
```

包会被下载到pkg/mod/github.com/haillk文件夹中去。

同时项目中出现新的文件go.sum

### 更新版本

```shell
go get github.com/haillk/testmod@v1.0.1
```

