---
title: "sync.Map的相关方法"
date: 2020-02-11T20:18:46+08:00
lastmod: 2020-02-11T20:18:46+08:00
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

### 核心源码：

```go
import(
	"sync/atomic"
    "unsafe"
)
//核心数据结构
//删除不是直接删除，而是直接打标记，是有在晋升dirty的时候才清理删除的数据。
type Map struct{
    mu mutex
    //read包含map中可以被安全并发访问的内容，read总是可以被安全load，但是只能和mu一起store
    //read中的数据可以不用mu锁就并发更新，但是要修改之前expunged的数据就需要将数据复制dirty区，
    read atomic.Value
    //dirty  包含map中需要持有mu锁的数据。
    //它包含所有read中expunged和unexpunged的数据。新增加的entries会加入到dirty.
    //删除的entries 没有储存在dirty map 中。
    dirty map[interface{}]* entry//包含最新写入的数据，当，misses计数达到一定值，将其值赋给read
    misser int// 计数作用，每次从read中读取失败，则计数加1
}
//readonly
type readonly struct{
    m map[interface{}]* entry 
    amended bool //map.dirty的数据和这里的m中的数据不一样的时候，为true
    //如果read找不到数据的时候会在dirty中找。
}
//entry 这里map.dirty和readonly.m中的数据是冗余的，但是他们储存的指针，指向了同一个数据，所以并没有占用太多的空间。
type entry struct{
    p unsafe.Pointer
    //p 有三种值，nil ：entry已经被删除了，并且dirty为nil
    // expunged ：entry已经被删除了，并且dirty不为nil，而且这个entry不存在于dirty中
    //正常值
}

```

### 使用方法：

```go
//Load 查找
//store 
//delete
//range
```

//append不是安全方法