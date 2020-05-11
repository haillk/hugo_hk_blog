---
title: "Select作用"
date: 2019-12-24T17:26:06+08:00
lastmod: 2019-12-24T17:26:06+08:00
show_in_homepage: true

tags: []
categories: ['GoLang']

featured_image: ''
featured_image_preview: ''

comment: true
toc: false
autoCollapseToc: true
---

<!--more-->

## select

	>select会从多个发送或者接收信道进行选择，语句会阻塞到其中有信道可以操作，如果有多个信道可以操作，会随机选择其中一个case执行。

示例：

```go
// main
package main

import (
	"fmt"
	"time"
)

func service1(ch chan string) {
	time.Sleep(2 * time.Second)
	ch <- "from service1"
}
func service2(ch chan string) {
	time.Sleep(1 * time.Second)
	ch <- "from servie2"
}
func main() {
	ch1 := make(chan string)
	ch2 := make(chan string)
	go service1(ch1)
	go service2(ch2)
	select {
	case s1 := <-ch1:
		fmt.Println(s1)
	case s2 := <-ch2:
		fmt.Println(s2)
	default:
		fmt.Println("default")
	}
}

```

同时监听三个信道，哪个先准备好就执行哪一个。上例中，1,2都有准备时间，所以default不需要准备就先输出。

notes1.  如果监听的信道为nil，select就会忽略这个信道。而空的select会造成阻塞。

notes2. 有时候不希望立即执行default语句，而是希望等待一段时间，那么可以在case后面设置超时时间。

示例：

```go
case <-time.After(2*time.Second):
	fmt.Println("replace default")
```

