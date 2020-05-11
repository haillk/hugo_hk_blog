---
title: 'Defer原理'
tags: 
-
categories: 
- GoLang
date: "2020-05-09T15:38:03+08:00"
comment:  true    
---

佛门广大，岂不容一癫僧！

<!--more-->

### defer规则1：再defer声明时，其参数已经被加载

```go
func main() {
	a, b := 1, 2
	defer ff(a, b)
	a = 0
	fmt.Print("in main")
}
func ff(a, b int) {
	c := a + b
	fmt.Print("in func", c)
}


#output
in mainin func3
```

- defer后a的更改并没有影响到defer中a的值。

### defer规则2：defer声明和返回值的加载顺序

```go
func main() {
    v := c()
    fmt.Println(v)
}

func c() (i int) {
    defer func() { i++ }()
    return 1

}

#output
2
```

- 先计算return 的值RET，再计算defer，最后返回RET

### 从实例分析原理：

```go
go build -gcflags=-l -o defer main.go
go tool objdump -s "main\.main" defer

TEXT main.main(SB) H:/GoProgramming/src/EX/main.go
  main.go:7             0x49a7d0                65488b0c2528000000              MOVQ GS:0x28, CX
  main.go:7             0x49a7d9                488b8900000000                  MOVQ 0(CX), CX
  main.go:7             0x49a7e0                488d842458ffffff                LEAQ 0xffffff58(SP), AX
  main.go:7             0x49a7e8                483b4110                        CMPQ 0x10(CX), AX
  main.go:7             0x49a7ec                0f8697010000                    JBE 0x49a989
  main.go:7             0x49a7f2                4881ec28010000                  SUBQ $0x128, SP
  main.go:7             0x49a7f9                4889ac2420010000                MOVQ BP, 0x120(SP)
  main.go:7             0x49a801                488dac2420010000                LEAQ 0x120(SP), BP
  main.go:8             0x49a809                0f57c0                          XORPS X0, X0
  main.go:8             0x49a80c                0f11842400010000                MOVUPS X0, 0x100(SP)
  main.go:8             0x49a814                488d0525280100                  LEAQ 
………………
runtime.deferprocStack(SB)
  main.go:8             0x49a885                85c0                            TESTL AX, AX
  main.go:8             0x49a887                0f85e6000000                    JNE 0x49a973
  main.go:9             0x49a88d                0f57c0                          XORPS X0, X0
  main.go:9             0x49a890                0f118424f0000000                MOVUPS X0, 0xf0(SP)
  main.go:9             0x49a898                488d05a1270100                  LEAQ …………
………………
runtime.deferprocStack(SB)
  main.go:9             0x49a8f7                85c0                            TESTL AX, AX
  main.go:9             0x49a8f9                7562                            JNE 0x49a95d
  main.go:10            0x49a8fb                0f57c0                          XORPS X0, X0
  main.go:10            0x49a8fe                0f11842410010000                MOVUPS X0, 0x110(SP)
  main.go:10            0x49a906                488d0533270100                  LEAQ 
………………
runtime.deferreturn(SB)
  main.go:11            0x49a94d                488bac2420010000                MOVQ 0x120(SP), BP
  main.go:11            0x49a955                4881c428010000                  ADDQ $0x128, SP
  main.go:11            0x49a95c                c3                              RET

  main.go:9             0x49a95d                90                              NOPL

  main.go:9             0x49a95e                e87d1af9ff                      CALL runtime.deferreturn(SB)
  main.go:9             0x49a963                488bac2420010000                MOVQ 0x120(SP), BP
  main.go:9             0x49a96b                4881c428010000                  ADDQ $0x128, SP
  main.go:9             0x49a972                c3                              RET

  main.go:8             0x49a973                90                              NOPL

  main.go:8             0x49a974                e8671af9ff                      CALL runtime.deferreturn(SB)
  main.go:8             0x49a979                488bac2420010000                MOVQ 0x120(SP), BP
  main.go:8             0x49a981                4881c428010000                  ADDQ $0x128, SP
  main.go:8             0x49a988                c3                              RET

  main.go:7             0x49a989                e8e286fbff                      CALL runtime.morestack_noctxt(SB)
  main.go:7             0x49a98e                e93dfeffff                      JMP main.main(SB)

H:\GoProgramming\src\EX>
```

代码中使用了两次defer. 汇编代码中也就使用了两次deferproc和deferreturn



[源码地址](https://github.com/golang/go/blob/release-branch.go1.12/src/runtime/panic.go#L229)

[参考](https://zhuanlan.zhihu.com/p/99303379)

### 编译调用源码：

```go
func deferproc(){
    获取当前goroutine
    申请一个结构体存放defer相关数据
    …………
}
```

```go
func deferreturn(arg0 uintptr) {
  gp := getg() // 获得当前的goroutine
  d := gp._defer
  if d == nil { // 如果没有defer函数，直接return
    return
  }
  ...
  fn := d.fn // 获得defer的func函数
  d.fn = nil // 重置
  gp._defer = d.link // 将前一个defer函数attach到当前goroutine
  freedefer(d) // 释放defer函数
  _ = fn.fn // 执行defer的func函数
  jmpdefer(fn, uintptr(unsafe.Pointer(&arg0)))
}
```

### defer结构体源码：

```go
type _defer struct {
  siz int32
  started bool
  heap bool
  openDefer bool
  sp uintptr // 栈指针
  pc uintptr // 调用方的程序计数器
  fn *funcval // 传入的函数
  _panic *_panic
  link *_defer // 指向下一个执行的defer函数
  fd unsafe.Pointer
  varp uintptr
  framepc uintptr
}
```

### newdefer源码：

```go
func newdefer(siz int32) *_defer {
  var d *_defer
  sc := deferclass(uintptr(siz))
  gp := getg() // 获得当前的goroutine
  ...
  d.link = gp._defer // 现在新的defer函数的link指向了当前的defer函数
  gp._defer = d // 新的defer函数现在是第一个被调用的函数了
  return d
}
```

### 总结

defer对象是以链表的的形式关联到goroutine上的，每添加一个defer，就在链表顶端添加这个defer.

