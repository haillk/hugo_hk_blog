---
title: 'Slice结构'
tags: 
-
categories: 
- GoLang
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

### Slice指针:

```go
s:=make([]byte, 200)
ptr:=unsafe.Pointer(&s)
```

&s取到的是Slice结构体的指针。使用uintptr操作的话，下一个取到的是cap和len

&s[0]取道德是s[0]的指针。使用uintptr取到的是s[1]

### cap函数

对切片使用cap函数，取到的是从切片头开始到原始数组的末尾

### 扩容

如果slice的容量小于1024,则直接翻倍

如果超过1024个元素，增长因子编程1.25

如果新的容量在旧的容量两个以上，则会直接扩容到新容量大小

### 修改

对slice的修改会直接作用在原始数组上。

### 函数传参和赋值

函数传参和赋值都是浅拷贝

公用一个底层数组，但是len和cap是各自独立的。

问题，如果多个slice对同时对底层数组进行操作，只有最后一个会生效。

### 空切片

`s:=make([]int ,0,0)`

空切片中指针指向了一个zerobase的特殊指针。

### nil切片

`var s []int`

指针指向了nil