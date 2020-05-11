---
title: 'Go_Channel详解'
tags: 
-
categories: 
- GoLang
date: "2020-04-01T14:58:14+08:00"
comment:  true    
---

*每天都有新体验

<!--more-->

### Channel类型：

`Channel`的类型有三种，<-指明方向，如果没有指明方向，那就是双向的。

```go
chan T          // 可以接收和发送类型为 T 的数据
chan<- float64  // 只可以用来发送 float64 类型的数据
<-chan int      // 只可以用来接收 int 类型的数据
```

`<-`总是优先和最左面的类型结合。

```go
chan <- chan int  //等价于chan<-(chan int)
chan <- <- chan int //等价于chan <- (<-chan int)
<- chan <- chan int //等价于 <-chan (<-chan int)
```

### 创建Channel

`make (chan int ,100)` 100是容量（capacity）.如果容量为零，就是没有缓存。只有sender和receiver都准备好了之后他们的通讯才会开始。如果设置了缓存，只有buffer慢了之后send才会阻塞，而只有缓存空了之后，receive才会阻塞。一个nil channel不会通信。

- 可以在多个goroutine中同时往一个channel中receive/send数据，不必考虑额外的同步措施。
- channel是一个先进先出队列。
- recevie支持multi-valued assignment `v,ok:=<-ch` 可以检查channel是否关闭。

### send/receive

##### send：

`c:=make(chan int) c<-3`send向channel中发送数据。

- 在通讯开始前，expression必需先求出值来。

  - ```go
    c := make(chan int)
    defer close(c)
    go func() { c <- 3 + 4 }()
    i := <-c
    fmt.Println(i)
    ////out
    7
    ```

- send被执行前通讯一直被阻塞，无缓存的channel只有在receive准备好后才会被执行。
- 向一个已经被关闭的channel中发送数据会导致`run-time panic`
- 向`nil channel`中发送数据会一直被阻塞

##### receive:

`<-ch`用来从channel中接受数据，这个表达式会一直阻塞，直到有数据可以接收。

- 从一个`nil channel`中接收数据会一直被阻塞
- 从一个关闭的channel中接受数据不会被阻塞，而是立即返回对应元素的零值。

### Range处理

示例

```go
c := make(chan int)
	go func() {
		for i := 0; i < 10; i = i + 1 {
			c <- i
		}
		close(c)
	}()
	for i := range c {
		fmt.Println(i)
	}

```

- range c产生的迭代值为channel中发送的值，替代了`<-ch`
- 这个`for range `会一直迭代直到channel被**关闭**。

###  select

示例：

```go
func fibonacci(c, quit chan int) {
	x, y := 0, 1
	for {
		select {
		case c <- x:
			x, y = y, x+y
		case <-quit:
			fmt.Println("quit")
			return
		}
	}
}
func main() {
	c := make(chan int)
	quit := make(chan int)
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-c)
		}
		quit <- 0
	}()
	fibonacci(c, quit)
}
```

- select选择一组可能的send或者receice操作处理。
- 如果同时多个channel不被阻塞，则会随机选择一个case处理
- 没有能处理case则选择defaulf处理，如果没有设置`dafault case`则一直被阻塞

### timeout

利用select可以很容易的设置超时。

```go
select{
    case res:=<-c1:
    fmt.println(res)
    case <-time.After(time.Second*1):
    fmt.println("timeout 1")
}
```



### Timer和Ticker

`timer:=time.NewTimer(time.second*2)`

```go
ticker := time.NewTicker(time.Millisecond * 500)
go func() {
	for t := range ticker.C {
		fmt.Println("Tick at", t)
	}
}()
```

- newtime相当于延时
- ticker相当于一个缓存channel,以等间隔时间发送数据



