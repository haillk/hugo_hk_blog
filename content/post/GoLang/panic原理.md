---
title: 'Panic原理'
tags: 
-
categories: 
- GoLang
date: "2020-05-09T17:38:51+08:00"
comment:  true    
---

佛门广大，岂不容一癫僧！

<!--more-->

### panci结构体源码：

[源码地址]( https://github.com/golang/go/blob/b89b4623eb70cbdc6b0aea43a5a826b7a26f20a7/src/runtime/runtime2.go#L882)

[参考](https://zhuanlan.zhihu.com/p/99303379)

```go
type _panic struct {
  argp unsafe.Pointer // 指向defer函数
  arg interface{} // panic时的参数
  link *_panic // 指向之前的panic
  pc uintptr
  sp unsafe.Pointer
  recovered bool           // 表示当前的panic是否被recover恢复
  aborted bool           // 当前panic是否被强行终止
  goexit bool
}
```

### 编译过程源码：

```go
func gopanic(e interface{}){
    gp:=getg()
    先处理defer
    
    在处理recover
}
```



### 总结：

panic在执行时会首先执行defer.

panic 也是关联在goroutine上的链表

### 有Recover的情况：

先处理defer,如果有recover,则设置recover的标志位为true

然后panic遇到recover后,  获取pc和栈指针sp,  并调用recover函数

recover函数：恢复pc,sp 等参数，调用gogo(&go.sched)

这里的gogo就是跳转回deferproc,从而回到正常逻辑。



