---
title: 'UnsafePointer'
tags: 
-
categories: 
- GoLang
date: "2020-05-08T19:58:52+08:00"
comment:  true    
---

佛门广大，岂不容一癫僧！

<!--more-->

### unsafe包

unsafe包包括：

```
func Alignof(x ArbitraryType) uintptr
func Offsetof(x ArbitraryType) uintptr
func Sizeof(x ArbitraryType) uintptr
type ArbitraryType
type Pointer
```

这篇post中主要分析`Pointer`

### Pointer

pointer主要有四个特征：

```
任何类型的指针都可以被转化为pointer
pointer可以被转换为任意类型的指针
uintptr可以被转换为pointer
pointer可以被转换为uintptr
```

> **uintptr是什么呢：**
>
> uintptr是golang的内置类型
>
> `typedef unsigned long long int  uint64;`
>
> `typedef uint64          uintptr;`
>
> 将Pointer转换为uintptr会生成所指向的值的内存地址（整数）。

1. 将`*T1`转换成`*T2` ，T2小于等于T1,并且有相同的内存分布

   ```go
   func f2i(f float64) uint64 {
   	return *(*uint64)(unsafe.Pointer(&f))
   }
   func main() {
   	fmt.Print(f2i(32.1))
   }
   
   #out
   4629714490685705421
   ```

2. 将指针转换成`uintptr`

uintptr是整数，而不是引用。 将Pointer转换为uintptr会创建一个没有指针语义的整数值。 

即使uintptr拥有某个对象的地址，垃圾回收器也不会在对象移动时更新该uintptr的值，该uintptr也不会使该对象被回收。

通常，将uintptr转换回Pointer无效。

3. 使用`uintptr`操作指针

```go
p=unsafe.Pointer(uintptr(p)+offset)
```

**note：**uinptr不能储存到一个变量中,必须在一行中操作。

```go
func main() {
	a := []int{1, 2, 3}
	p := uintptr(unsafe.Pointer(&a))
	u := unsafe.Pointer(p + 1)
	fmt.Print(u)
}

#output
0xc00009ff39
```

```go
func main() {
	a := []int{1, 2, 3}
	//p := uintptr(unsafe.Pointer(&a))
	u := unsafe.Pointer(uintptr(unsafe.Pointer(&a)) + 1)
	fmt.Print(u)
}

#output
0xc0000044a1
```

