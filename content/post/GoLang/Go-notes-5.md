---
title: "Go Notes 5"
date: 2019-12-21T17:37:28+08:00
lastmod: 2019-12-21T17:37:28+08:00
description: ""
show_in_homepage: true
show_description: false
license: ''

tags: []
categories: ['GoLang']

featured_image: ''
featured_image_preview: ''

comment: true
toc: true
autoCollapseToc: true
math: false
---

<!--more-->

## <font color=#cc3300>进制数，零开头的数字会被识别为8进制</font>

示例

```go
package main

import (
	"fmt"
)

func main() {
	de := 010
	fmt.Println(de)

}
```

结果是 8    虽然十个小知识点，有时候还是会记不住。

## <font color=#cc3300>返回error类型时 err==nil?吗---error也属于接口值的判断</font>

问题：下列代码输出什么？

```go
func Foo() error {
	var err *os.PathError = nil
	return err
}
func main() {
	err := Foo()
	fmt.Println(err)
	fmt.Println(err == nil)
}
```

输出结果竟然是

```go
H:/GoProgramming/src/src.exe  [H:/GoProgramming/src]
<nil>
false
```

**原因：只有在值和动态类型都是nil的时候，接口值才为nil。Foo函数返回的err动态类型为*os.PathError,值为nil。所以err!=nil**

解决方法：如果就是想判断返回的err的值是否为nil呢。

```go
func Foo() (err error) {

	return err
}
···
fmt.Println(err == nil)
```

直接声明返回值的类型为errror就能直接判断了

## <font color=#cc3300>Named type 和 UnNamed Type(具名类型和非具名类型)</font>

先看一个现象：

```go
type T int
func F(t T){}
func main(){
    var q int
    F(q)
}
```

```go
type T []int
func F(t T){}
func main(){
    var q []int
    F(q)
}
```

**结果：第一个编译错误，而第二个能编译通过**

>我们知道，不同类型的值是不能相互赋值的，即使底层类型相同，所以第一个编译不能通过
>
>还有，对于**底层类型相同的变量可以相互赋值还有一个条件，即至少有一个不是具名变量**。

所以只要有一个不是具名变量，那么底层类型相同的变量就能相互赋值了。

所以在第二个程序中  虽然T是具名类型的变量，但是[]int不是，这才能相互赋值。

#### 那么哪些才是具名类型呢。

>- 内置类型，比如int int64 float string bool等
>- 使用type声明的类型

#### 什么有是UnNamed Type呢

> 基于已有的Named type组合在一起的类型，比如strcut{}, []string ,interface{} ,map等

------

#### 类型添加方法

再思考一个问题，在为类型添加方法的时候，Unnamed Type和Named Type有什么区别吗。

```go
func (m map[string]string)Set(key string,value string){
    map[key] = value
}
func main(){
    m := make(map[string]string)
    m.Set("A","one")
}
```

上面代码编译是通不过的，就是因为map是UnNamed Type所以不能添加方法。

解决方法：

``` go
type T map[string]string
```

通过type关键字，把map转换为具名类型。