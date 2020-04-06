---
title: "跟着无闻学Go Web应用开发 简单模板"
date: 2019-12-18T23:23:20+08:00
lastmod: 2019-12-18T23:23:20+08:00
description: ""
show_in_homepage: true
show_description: false
license: ''

tags: ['go','web']
categories: ['跟着无闻学Go-web应用开发']

featured_image: ''
featured_image_preview: ''

comment: true
toc: true
autoCollapseToc: true
math: false
---

<!--more-->

标准库中的 `text/template` 包是 Go 语言内置的文本模板引擎.

## <font color=#cc3300>模板渲染步骤分析</font>

还是从hello world程序开始分析。

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"text/template"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		// 创建模板对象并解析模板内容
		tmpl, err := template.New("test").Parse("Hello world!")
		if err != nil {
			fmt.Fprintf(w, "Parse: %v", err)
			return
		}

		// 调用模板对象的渲染方法
		err = tmpl.Execute(w, nil)
		if err != nil {
			fmt.Fprintf(w, "Execute: %v", err)
			return
		}
	})

	log.Println("Starting HTTP server...")
	log.Fatal(http.ListenAndServe("localhost:4000", nil))
}
```

过程分析：

1. template.New的作用是根据名称创建一个模板对象
2. template.Parse 方法接受一个string参数，即文本模板的内容，然后对内容进行解析，并返回结果。
3. template.Execute （io.wirter,interface{}）就是渲染模板的方法。有两个参数（输出对象和指定数据对象）。第一个参数，凡是实现了io.Writer接口的实例均可作为输出对象。本例就是w.。第二个参数，是根对象，即渲染时需要的数据,可以接受任何类型。

##  <font color=#cc3300>在模板中渲染变量</font>

首先看一些怎样获取http协议中get请求的url查询参数。

```go
package main

import (
	"log"
	"net/http"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte(r.URL.Query().Get("val")))
	})

	log.Println("Starting HTTP server...")
	log.Fatal(http.ListenAndServe("localhost:4000", nil))
}
```

然后结合模板操作

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"text/template"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		// 创建模板对象并解析模板内容
		tmpl, err := template.New("test").Parse("The value is: {{.}}")
		if err != nil {
			fmt.Fprintf(w, "Parse: %v", err)
			return
		}

		// 获取 URL 参数的值
		val := r.URL.Query().Get("val")

		// 调用模板对象的渲染方法
		err = tmpl.Execute(w, val)
		if err != nil {
			fmt.Fprintf(w, "Execute: %v", err)
			return
		}
	})

	log.Println("Starting HTTP server...")
	log.Fatal(http.ListenAndServe("localhost:4000", nil))
}
```

模板中的｛｛.｝｝ 中  点操作符默认只想跟对象，即我们的调用template.Execute方法时传入的第二个参数，本例中跟对象时一个单纯的string类型变量nal。点操作符渲染的就是我们的变狼val.

当然也可以渲其它类型map ,结构体，方法都可以。