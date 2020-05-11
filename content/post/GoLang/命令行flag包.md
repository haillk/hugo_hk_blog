---
title: "命令行flag包"
date: 2020-02-19T22:35:53+08:00
lastmod: 2020-02-19T22:35:53+08:00
show_in_homepage: true

tags: ['命令行']
categories: ['GoLang']

featured_image: ''
featured_image_preview: ''

comment: true
toc: false
autoCollapseToc: true
---

<!--more-->

### Flag包

​	go提供了flag包,可以方便的操作命令行参数。

使用方法：

```go
package main
//导入flag包
import (
	"flag"
)
//定义要使用的变量
var (
	h bool
    s string
    q bool
)
func init(){
    //两种绑定方式（指针，名字，默认值，提示信息）
    flag.BoolVar(&h,"hname",false,"帮助信息")
    var z = flag.Bool(&q,false,"退出程序")  //返回相应的指针
    //使用parse 调用
    flag.Parse()
}
```

命令行语法有三种形式 

​	-flag            只支持bool

​	-flag=x        只支持非bool

​	-flag x         只支持非bool

当遇到不符合规格的参数时，会停止解析。想不符合规格的参数被称为non-flag参数

>  Arg(i int)和Args()这两个方法就是获取non-flag参数的；NArg()获得non-flag个数；NFlag()获得FlagSet中actual长度（即被设置了的参数个数）

flag.Args能获取参数。