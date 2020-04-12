---
title: 'Slice结构'
tags: 
-
categories: 
- Golang
date: "2020-04-12T15:22:35+08:00"
comment:  true    
---

前进！前进！！不择手段的前进！

<!--more-->

### Slice的数据结构  [参考slice](https://halfrost.com/go_slice/)

```go
type slice struct{
    array unsafe.Pointer
    len int
    cap int
}
```

整体的结构是这样的

![](https://pic.downk.cc/item/5e92c26ec2a9a83be513a679.jpg)

这个还比较好理解：如图所示。。

### 使用数组指针:

```go
s:=make([]byte, 200)
ptr:=unsafe.
```

