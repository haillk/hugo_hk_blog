---
title: "SyncMutex互斥锁"
date: 2019-12-27T00:28:48+08:00
lastmod: 2019-12-27T00:28:48+08:00
show_in_homepage: true

tags: []
categories: ['GoLang']

featured_image: ''
featured_image_preview: ''

comment: true
toc: false
autoCollapseToc: true
---

<!--more-->

## Sync.mutex的使用

Go语言的sync包中提供了两种锁类型，sync.mutex和sync.RWmutex，前者是互斥锁，后者是读写锁。

- 使用Lock()加锁后，不能再继续对其加锁（同一个goroutine中，即：同步调用），否则会panic。只有在unlock()之后才能再次Lock()。异步调用Lock()，是正当的锁竞争，当然不会有panic了。适用于读写不确定场景，即读写次数没有明显的区别，并且只允许只有一个读或者写的场景，所以该锁也叫做全局锁。
- func (m *Mutex) Unlock()用于解锁m，如果在使用Unlock()前未加锁，就会引起一个运行错误。已经锁定的Mutex并不与特定的goroutine相关联，这样可以利用一个goroutine对其加锁，再利用其他goroutine对其解锁。

示例：

```go
func main() {
	var mutex sync.Mutex
	mutex.Lock()
	go func() {
		mutex.Lock()
		fmt.Println("test")

		time.Sleep(time.Second)
		mutex.Unlock()
	}()

	mutex.Unlock()
	time.Sleep(time.Second)
}

```

用法与其他语言没什么太大不同。

## 作为struct的一部分来使用

作为structd的一部分来使用，这样这个struct 就会防止被多线程更改数据。

```go
package main

import (
	"fmt"
	"sync"
	_ "time"
)

type Book struct {
	bookname string
	sync.Mutex
}

func (bb *Book) SetName(wg *sync.WaitGroup, name string) {
	bb.Lock()
	defer bb.Unlock()
	fmt.Println("tes1", name)
	bb.bookname = name
	fmt.Println("tes1", name)
	wg.Done()

}
func main() {
	wg := &sync.WaitGroup{}
	wg.Add(3)
	bk := Book{}
	books := []string{"111", "222", "3333"}
	for _, bookname := range books {
		go bk.SetName(wg, bookname)
	}
	wg.Wait()
}
```

