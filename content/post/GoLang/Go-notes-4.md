---
title: "Go Notes 4"
date: 2019-12-06T23:25:31+08:00
categories:
- GoLang
tags:
- 
keywords:
- golang
- recover  
---

recover的扑捉，defer函数的接受者和参数

<!-- more -->

#### <font color=#CC3300>1、recover必须隔一个函数帧栈才能捕获到</font>

```go
//错误
func main() {
	recover()
	panic(1)
}
//错误
func main() {
	defer recover()
	panic(1)
}
//正确
func main() {
	defer func() {
		recover()
	}()
	panic(1)
}
//错误
func main() {
	defer func() {
		defer func(){
			recover()
		}
		
	}()
	panic(1)
}
```

#### <font color=#CC3300>2、defer函数的参数和接受者是在defer语句出现的位置做计算的</font>

```go
func main(){
    defer s.Add(1).Add(2)
    s.Add(3)
}
//调用顺序
s.add(1)-->s.Add(3)-->s.Add(2)
```

#### 

