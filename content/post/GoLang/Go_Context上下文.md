---
title: 'Go_Context上下文'
tags: 
-
categories: 
- GoLang
date: "2020-05-11T21:52:44+08:00"
comment:  true    
---

佛门广大，岂不容一癫僧！

<!--more-->

### 使用channel控制goroutine

传统的使用channel控制goroutine的结束

```go
ch:=make(chan int)
go func(){
    <-ch
}
ch<-1
```

缺点：如果有很多个goroutine需要结束，或者很多嵌套goroutine。使用这种方法就很复杂了。

### 使用Context控制多个goroutine

```go
package main

import (
	"context"
	"fmt"
	"time"
)

func main() {
	ctx, canel := context.WithCancel(context.Background())
    //Context.Backgroud(),作为context树结构的顶层。
    //有了顶层之后，就可以衍生出更多的子Context.
    //
	go watch(ctx, "1")
	go watch(ctx, "2")
	go watch(ctx, "3")
	time.Sleep(10 * time.Second)
	fmt.Println("要结束了")
	canel()
	time.Sleep(5 * time.Second)

}
func watch(ctx context.Context, name string) {
	for {
		select {
		case <-ctx.Done():
			fmt.Println(name, "stop")
			return
		default:
			fmt.Println(name, "running")
			time.Sleep(2 * time.Second)
		}

	}
}

```

```go
2 running
1 running
3 running
3 running
2 running
1 running
1 running
2 running
3 running
1 running
3 running
2 running
2 running
1 running
3 running
要结束了
1 stop
2 stop
3 stop
```

具体的就是使用ctx替换上文的channel

### Context接口

```go
type Context interface{
    Deadline()(deadline time.Time,ok bool)#获取设置的截止时间，ok为false的话代表没有设置
    Done() <-chan struct{}#返回一个只读的channel，读取到chann则代表接受到了退出请求
    Err() err#返回取消的原因
    Value(key interface{}) interface{}#返回绑定在Context上的值
}
```

### Context衍生

context包为我们提供了with系列的函数。

```go
func WithCancel(parent Context) (ctx Context, cancel CancelFunc)
func WithDeadline(parent Context, deadline time.Time) (Context, CancelFunc)
func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)
func WithValue(parent Context, key, val interface{}) Context
```

前三个都返回一个取消函数

deadline和timeout分别是到相应的时间点和设置超时时间来取消。

第4个生成绑定了一个键值对的Context.

### Context.Value

使用context.Value就是用继承的方法，加上一个withValue，就可以用ctx.Value获取到值。

值必须是可比的，value是线程安全的，一般是值。

```go
    ctx, cancel := context.WithCancel(context.Background())
	//附加值
	valueCtx:=context.WithValue(ctx,key,"【监控1】")
	go watch(valueCtx)
 ………………
    print(ctx.Value(key))
```

### 使用

1. 不要把Context放在结构体中，要以参数的方式传递
2. 以Context作为参数的函数方法，应该把Context作为第一个参数，放在第一位。
3. 给一个函数方法传递Context的时候，不要传递nil，如果不知道传递什么，就使用context.TODO
4. Context的Value相关方法应该传递必须的数据，不要什么数据都使用这个传递
5. Context是线程安全的，可以放心的在多个goroutine中传递