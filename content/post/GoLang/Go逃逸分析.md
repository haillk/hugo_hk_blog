---
title: 'Go逃逸分析'
tags: 
-
categories: 
- GoLang
date: "2020-04-04T18:17:41+08:00"
comment:  true    
---

前进！前进！！不择手段的前进！

<!--more-->

### 堆栈：

栈的分配内存秩序呀两个cpu指令，`push 和release` 分配和释放。

堆的分配内存需要先找到一块大小合适的内存块，之后要通过垃圾回收才能释放。

### 逃逸分析：

<u>Go是通过在编译器里做**逃逸分析**，不逃逸的对象放在栈上，可能逃逸的放在堆上。</u>

即发现**变量**在退出函数后没有用了就放在栈上（栈上内存分配和回收都比堆上快的多）。

反之如果函数内的普通变量经过逃逸分析后，发现函数在退出后，**变量**还在其他地方有引用，**就将变量**

分配在堆上。

**为什么要逃逸分析**

1. 减少gc的压力，栈上的变量，随着函数退出后系统直接回收。不需要gc标记回收
2. 减少内存碎片的压力
3. 减轻分配堆内存的开销，提高程序的运行速度。

### 如何确定是否逃逸

`go run -gcflags '-m -l' main`

`-m`打印逃逸分析的优化策略，实际上可一用4个，

`-l`会禁用函数内联，

实例 **取地址发生逃逸。**

```go
func ff() *string {
	name := "haillk_iii"
	return &name
}
func ffq() string {
	name := "haillk_name"
	return name
}
func main() {
	//tset := "faf"
	fmt.Println(*ff())
	fmt.Println(ffq())

}
```

```go
#output
.\main.go:8:2: moved to heap: name
.\main.go:17:13: main ... argument does not escape
.\main.go:17:14: *ff() escapes to heap
.\main.go:18:13: main ... argument does not escape
.\main.go:18:17: ffq() escapes to heap
haillk_iii
haillk_name
```

函数ff() 的返回值是个指针，可能在函数外使用，所以就逃逸了。

**那问题来了，编译器是通过函数返回值判断是否逃逸的嘛？**

还是不知道

### 返回泛型 逃逸：

栗子1：

```go
func ffq(nameq interface{}) {
	tset := nameq
	_ = tset
}
func main() {
	//tset := "faf"
	namem := "haillk"
	ffq(namem)

}

```

```go
#output
.\main.go:7:10: ffq nameq does not escape
.\main.go:14:5: main namem does not escape
```

并没有逃逸呀，返回试试看

```go
func ffq(nameq interface{}) interface{} {
	tset := nameq
	return tset
}
func main() {
	//tset := "faf"
	namem := "haillk"
	ffq(namem)

}
```

```go
.\main.go:7:10: leaking param: nameq to result ~r1 level=0
.\main.go:14:5: main namem does not escape
```

这就逃逸了。emmmm 当**然只有返回泛型才会逃逸。**

### 结构体中涉及的逃逸

例子：

```go
type TT struct {
	name string
	age  int
}

func main() {

	var t1 = new(TT)
	t1.age = 12
	t1.name = "hia"

}
```

```go
.\main.go:14:14: main new(TT) does not escape
```

这说明结构体中没有引用时，不会发生逃逸

但如果包含了引用，

例子：

```go
type TT struct {
	name string
	age  *int
}

func main() {
	//tset := "faf"
	var t1 = &TT{}
	t1.age = new(int)
	t1.name = "hia"

}
#outpput
.\main.go:14:11: main &TT literal does not escape
.\main.go:15:14: new(int) escapes to heap
```

这是因为编译器分析，对象`t1`可能分配在堆上，为了防止栈回收后，导致成员变量也被回收，就要把`age`也逃逸到堆上。

### map,chan slice中存在引用，逃逸

```go
func main() {
	c := make(map[*int]int)
	b := 12
	c[&b] = 2
	_ = c

}
#output
.\main.go:9:2: moved to heap: b
.\main.go:8:11: main make(map[*int]int) does not escape
```

应用了`b`,`b`就逃逸了