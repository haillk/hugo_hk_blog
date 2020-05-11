---
title: "Go Notes 2"
date: 2019-11-29T01:39:30+08:00
categories:
- GoLang
tags:
- 
keywords:
- tech
#thumbnailImage: //example.com/image.jpg
---

map中value是否可寻址的问题，数组的比较和切片的比较，存在全局变量和局部变量时，调用方法会读取全局变量，for range的次数和副本问题

<!--more-->

#### <font color=#CC3300>1、map中value是否可寻址的问题</font> ####

Eg.

```go
type myst struct{
    x,t int
}
var m = map[string]myst{
    "hai":myst{1,2}
}
func main(){
    fmt.Println(m["hai"].x)//成功
    m["hai"].x = 4//失败
}
```

​	对于 `map[string]Struct` 和 `map[string][]int`的问题，由于结构体struct是不可寻址的，虽然可以读取这个值，但是不能把值赋值给x。而[]int是可寻址的，不仅可以读取，也可以赋值。解决方法：使用临时变量或者使用可寻址value。
#### <font color=#CC3300>2、数组的比较和切片的比较</font> ####
>1、go中不同类型不能相互比较，而数组长度是数组类型的一部分，所以不同长度的数组不能比较
	2、切片不能比较

#### <font color=#CC3300>3、存在全局变量和局部变量时，调用方法会读取全局变量</font> ####

eg.

```go
var s int//全局变量
func extra(){
    fmt.Println("extra",s)
}
func main(){
    s :=3 //局部变量
    extra()
    fmt.Println("main",s)
}
```

会输出 `extra 0 , main 3`，则调用的方法会使用全局变量
#### <font color=#CC3300>4、for range的次数和副本问题</font> ####

Eg. for rang的次数问题

```go
func main(){
    v :=[]int{1,2,3}
    for i:=range v {
        v = append(v,i)
    }
}
```

> 不会出现死循环，循环次数在开始前就确定了

Eg. 副本问题

```go
//实际上是a的副本参与循环，不会输出999
for i,v:=range a{
    a[1] = 999
    fmt.Println(v)
}
//这才会让a亲自参与，输出999
for i,v :=rang &a{
    a[1] = 999
    fmt.Println(v)
}
```
#### <font color=#CC3300>5、方法的值接受者和指针接受者</font>

```go
type People struct{
    Name string
}
//值方法
func (p People)getName() string{
    return d.Name
}
//指针方法
func (p *People)SetName(name string){
    d.Name = name
}
```

>
>
>值类型只能调用值方法，指针类型既能调用值方法，也能调用指针方法。但是**<u>有时</u>**编译器会帮我们自动转义，所以下列语句都是可用的，不会报错。
>
>```go
>	p := People{"xiaoming"}
>	fmt.Println(p.getName())
>	//值类型只能调用值方法，此处是编译器自动转义，但是有特殊情况
>	p.SetName("xiaohong")
>	fmt.Println(p.getName())
>
>	//指针类型既能调用值方法，也能调用指针方法
>	pp := &p
>	pp.SetName("xiaolv")
>	fmt.Println(pp.getName())
>```
>
>```go
>emmmmm 我也有点疑惑，问题待定
>```
>
>

Notes：值方法操作的是接受者的副本。