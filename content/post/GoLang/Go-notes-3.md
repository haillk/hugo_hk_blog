---
title: "Go Notes 3"
date: 2019-11-30T03:32:08+08:00
categories:
- GoLang
tags:
- 
keywords:
- golang
- go语言注意点
#thumbnailImage: //example.com/image.jpg  
---

nil channel和已经关闭的channel,常量寻址，断言，cap,不能使用短变量声明设置结构体字段值，slice

<!--more-->

#### <font color=#CC3300>1、i++是语句，不是表达式</font>

​		i++ 和 i-- 在 Go 语言中是语句，不是表达式，因此不能赋值给另外的变量,j = 		i++是错误的

#### <font color=#CC3300>2、nil channel和已经关闭的channel</font>

1. 在nil channel 接受或者发送数据都会造成阻塞

2. 给一个已经关闭的channel发送数据，引起panic ,从一个已经关闭的channel接收数据，如果缓冲区为空，返回零值。

#### <font color=#CC3300>3、常量寻址问题</font>

​	1. 常量通常会被编译器在预处理阶段直接展开，作为指令数据使用，所以无法寻址。

	2. 变量在运行期间分配内存。



#### <font color=#CC3300>4、各种知识点</font>

- 只接受的 channel 不可以被关闭，发送的channel可以被关闭。

- 函数中声明的变量必须要使用，但可以有未使用的全局变量，函数的参数未使用也可以。

- 自定义的string方法中，不能再用string(包括springf方法)，会造成递归。

- 断言，`x:=interface{}(nil)  _,c:=x.(interface{})` 结果中，c 是false的，因为动态类型为nil,断言会失败。

- cap() 函数可以获取数组，数组指针，slice,channel,的容量，但不能获取 map.  可以用len()获取map的元素个数。

- slice不能比较，但是可以和nil比较

- x[2:] ,对于[i:j]j省略的时候，默认为len而不是cap
#### <font color=#CC3300>5、不能使用短变量声明设置结构体字段值</font>
```go
var data Struct//声明一个结构体
## 错误
data.re,err := 12,nil
## 正确
var err error
date.re,err = 12,nil
```










