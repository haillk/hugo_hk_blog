---
title: "Go Notes 1"
date: 2019-11-29T00:16:19+08:00
categories:
- GoLang
tags:
- 
comments: true
keywords:
- golang
- go语言注意点
#thumbnailImage: //example.com/image.jpg  
---

  多重赋值，短变量声明，interface{}和interface{}指针，Slice 扩容，基于类型创建的方法

<!--more-->

#### <font color=#CC3300>1、多重赋值</font> ####

​	先计算等号左边的索引表达式和取值表达式，再计算等号右边的表达式，最后赋值。

Eg.

```go
func main(){
    var k = 1
    var s = []int{1,2}
    k,s[k] = 0,3
    fmt.Println(s[0]+s[1])
}
```

先计算左边，k(1),s[1] ,再计算右边。结果k=0,s[1]=3

#### <font color=#CC3300>2、短变量声明</font> ####

​	当多值赋值时，:= 左边的变量无论声明与否都可以

Eg.

```go
var x int
x,y:=1,2
```

​	上面的代码是正常的

#### <font color=#CC3300>3、interface｛｝和interface指针</font> ####

Eg.

```go
var s Struct{}
var q &Struct{}
...
func f1(i interface{}){
    
}
func f2(qi *interface{}){
    
}
...
f1(s) f1(q) f2(s) f2(q)
```

​	f1(s)和f1(q)能通过编译，但f2(s),f2(q)不能通过编译。

> 函数参数为interface｛｝时可以接受任何类型的参数，包括用户自定义类型等，及时是接受指针类型也用interface{},而不是用*interface{}.
>
> 永远不要使用一个指针指向一个接口类型，因为他已经是一个指针。
> 

#### <font color=#CC3300>4、Slice扩容导致的问题</font> ####

Eg.

```go
func main(){
    s1 :=[]int {1,2,3}
    s2 :=s1[1:]
    s2[1] = 4
    fmt.Println(s1)
    s2 = append(s2,5,6,7)
    fmt.Println(s1)
}
```

​	S1 和s2 使用同一底层数组，一个改变会影响另一个。但是append之后会生成新的数组，不再相互影响

#### <font color=#CC3300>5、基于类型创建的方法</font> ####

Eg.

```go
//错误
func (i int )MyIntFunc(){
    ...
}
//正确
type Myint int
func (i Myint)MyIntFunc(){
    ...
}
```

​	基于类型创建的方法必须定义在同一个包里面，解决办法，可以定义一个新的类型

