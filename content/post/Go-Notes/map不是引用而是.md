---
title: "Map不是引用而是指针"
date: 2019-12-25T16:40:51+08:00
lastmod: 2019-12-25T16:40:51+08:00
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

首先阐明一个观点，map和channel不是引用变量。

示例：

```go
func fn (m map[int]int){
    m = make(map[int]int)
}
func main(){
    var m = map[int]int
    fn(m)
    fmt.Println(m==nil)
}
```

分析：如果map是引用变量，那么执行过后，m将不会是nil然而,m仍然还是nil，**所以，map不是应用。**

那map到底是什么呢？

**map是指向runtime.makemap结构体的指针。**

```go
//方法定义
func makemap(t *maptype,hint int64,h *hmap,bucket unsafe.Pointer) *hmap
```

