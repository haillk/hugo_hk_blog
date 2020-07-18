---
title: "跟着无闻学Go Web应用开发-初窥http服务器"
date: 2019-12-18T21:45:23+08:00
lastmod: 2019-12-18T21:45:23+08:00
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

# Hello world!

第一步，用go语言搭建一个http版的‘hello world’程序

```go
package main

import (
	"log"
	"net/http"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		w.Write([]byte("Hello world!"))
	})

	log.Println("Starting HTTP server...")
	log.Fatal(http.ListenAndServe("localhost:4000", nil))
}
```

### <font color=#cc3300>三个主要函数的简单分析</font>

 1. <font color=#DC143C>**http.handleFunc()** </font>函数的作用是建立路由映射。包含两个参数，第一个参数是指定的路由规则。第二个参数是，要调用的响应函数（这个函数必须符合函数签名`func(http.ResponseWriter, *http.Request)`）。
1. **<font color=#DC143C>`func(w http.ResponseWriter, r *http.Request) {}`</font>** 这个函数同样有两个参数，第一个参数是请求所对应的响应对象`http.ResponseWriter` ,包括响应码，响应头，响应体等。第二个对象就是请求对应的请求对象 `*http.Request`,包括http请求的所有信息。
     	    	1. `http.ResponseWriter` 我们就是通过调用这个对象的write方法来向响应体写入字符的。
      
2. **<font color=#DC143C>`http.ListenAndServe`</font>**的作用是启动http服务器，并监听发送到指定地址和端口号的http请求。包含两个参数，第一个参数就是地址和端口号。第二个参数

### <font color=#cc3300>http.handlefunc() 函数分析</font>

​	实际上`http.HandleFunc`是标准库提供的一种简便写法，他的第二个参数必须为指定格式，是因为在`http.HandleFunc`函数内部会将传入的绑定函数转化为类型<u>`http.Handler`,</u> 

​	而这个对象是go标准中的http请求处理器对象，该对象实现了`http.Handler`接口：

```go
type Handler interface {
    ServeHTTP(ResponseWriter, *Request)
}
```

**结论**：**http.HandleFunc的根本作用就是将一个函数转化为实现了`http.Handler`接口的类型（`http.Handler`）。  那么我们就可以自己创建一个类型并实现`http.Handler`接口。这样就可以替代handleFunc函数**

实现代码：

```go
package main

import (
	"log"
	"net/http"
)

func main() {
	http.Handle("/", &helloHandler{})

	log.Println("Starting HTTP server...")
	log.Fatal(http.ListenAndServe(":4000", nil))
}

type helloHandler struct{}

func (_ *helloHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("Hello world!"))
}
```

## <font color=#cc3300>Http.ListrenAndServe(":4000",nil)</font>函数分析

​	首先来分析一下第二个参数nil，根据函数声明表示，nil代替的是一个实现了`http.Handler`接口的对象。

```go
func ListenAndServe(addr string, handler Handler) error {...}
```

​	没错，这个handler就是跟上面一样，实现了handler接口的类型。我们可以直接使用。

```go
package main

import (
	"log"
	"net/http"
)

func main() {
	log.Println("Starting HTTP server...")
	log.Fatal(http.ListenAndServe("localhost:4000", &helloHandler{}))
}

type helloHandler struct{}

func (_ *helloHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("Hello world!"))
}
```

这个代码运行起来跟前面的是一样的，不过**不能方便的将路由规则和函数进行绑定。**

**结论一**： **ListenAndServe（）的第二个参数就是一个handler,能直接用但是不能提供路由绑定。**

## <font color=#cc3300>http.ServeMux函数分析</font>---对http.Handle剖析

通过对Hande的源码分析，Handle 是对`http.ServeMux`对象(`http.DefaultServeMux`)封装了一层。

```go
func Handle(pattern string, handler Handler) {
    DefaultServeMux.Handle(pattern, handler)
}
```

而这个`http,serveMux`是go语言的带有基本路由功能的服务复用器。除了用`http.HandleFunc`或`http.Handle`这类方法操作`http.DefaultServeMux`之外，也可以用`http.NewServeMux`来创建一个新的`http.ServeMux`对象。

```go
package main

import (
	"log"
	"net/http"
)

func main() {
	mux := http.NewServeMux()
	mux.Handle("/", &helloHandler{})
	log.Println("Starting HTTP server...")
	log.Fatal(http.ListenAndServe("localhost:4000", mux))
}

type helloHandler struct{}

func (_ *helloHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("Hello world!"))
}
```

**结论**：**`http.Handle`方法，本质上是对`DefaultServeMux`这个对象的操作。**

**那么我们就可以，自己创建一个`http.NewServeMux`方法来对`DefaultServeMux`操作。**

## <font color=#cc3300>ListenAndServe是否也是对DefaultServeMux对象的封装呢</font>

源码

```go
func ListenAndServe(addr string, handler Handler) error {
    server := &Server{Addr: addr, Handler: handler}
    return server.ListenAndServe()
}
```

虽然不是全局的封装，但也是用了http.server的对象。

**结论：我们可以使用server写一个自定义程度较高的程序。**

```go
package main

import (
	"log"
	"net/http"
)

func main() {
	mux := http.NewServeMux()
	mux.Handle("/", &helloHandler{})

	server := &http.Server{
		Addr:    ":4000",
		Handler: mux,
	}
	log.Println("Starting HTTP server...")
	log.Fatal(server.ListenAndServe())
}

type helloHandler struct{}

func (_ *helloHandler) ServeHTTP(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("Hello world!"))
}
```

