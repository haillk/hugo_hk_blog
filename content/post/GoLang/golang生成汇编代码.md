---
title: 'Golang生成汇编代码'
tags: 
-
categories: 
- 未分组
date: "2020-05-09T16:15:35+08:00"
comment:  true    
---

佛门广大，岂不容一癫僧！

<!--more-->

### 1、go tool compile

`go tool compile -N -l -S main.go`

> go tool compile -help获取细节

-N 关闭优化

-l 关闭内联

-S 打印组件清单

### 2、go tool objdump

首先编译程序`go tool compile -N -l main.go`

使用`go tool objdump main.o`反汇编出代码。

### 3、go build -gcflag -S

`go build -gcflags -S main.go`

### 总结

1和3生成的是过程中的汇编，最终的机器码的汇编可以用2生成。

### 实例：

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
  main.go:8             0x49a814                488d0525280100                  LEAQ runtime.rodata+73792(SB), AX
  main.go:8             0x49a81b                4889842400010000                MOVQ AX, 0x100(SP)
  main.go:8             0x49a823                488d0d560d0500                  LEAQ sync/atomic.CompareAndSwapUintptr.args_stackmap+376(SB), CX
  main.go:8             0x49a82a                48898c2408010000                MOVQ CX, 0x108(SP)
  main.go:8             0x49a832                c784249000000030000000          MOVL $0x30, 0x90(SP)
  main.go:8             0x49a83d                488d0dccfb0300                  LEAQ go.func.*+10(SB), CX
  main.go:8             0x49a844                48898c24a8000000                MOVQ CX, 0xa8(SP)
  main.go:8             0x49a84c                488d942400010000                LEAQ 0x100(SP), DX
  main.go:8             0x49a854                48899424c0000000                MOVQ DX, 0xc0(SP)
  main.go:8             0x49a85c                48c78424c800000001000000        MOVQ $0x1, 0xc8(SP)
  main.go:8             0x49a868                48c78424d000000001000000        MOVQ $0x1, 0xd0(SP)
  main.go:8             0x49a874                488d942490000000                LEAQ 0x90(SP), DX
  main.go:8             0x49a87c                48891424                        MOVQ DX, 0(SP)
  main.go:8             0x49a880                e80b15f9ff                      CALL runtime.deferprocStack(SB)
  main.go:8             0x49a885                85c0                            TESTL AX, AX
  main.go:8             0x49a887                0f85e6000000                    JNE 0x49a973
  main.go:9             0x49a88d                0f57c0                          XORPS X0, X0
  main.go:9             0x49a890                0f118424f0000000                MOVUPS X0, 0xf0(SP)
  main.go:9             0x49a898                488d05a1270100                  LEAQ runtime.rodata+73792(SB), AX
  main.go:9             0x49a89f                48898424f0000000                MOVQ AX, 0xf0(SP)
  main.go:9             0x49a8a7                488d0de20c0500                  LEAQ sync/atomic.CompareAndSwapUintptr.args_stackmap+392(SB), CX
  main.go:9             0x49a8ae                48898c24f8000000                MOVQ CX, 0xf8(SP)
  main.go:9             0x49a8b6                c744243030000000                MOVL $0x30, 0x30(SP)
  main.go:9             0x49a8be                488d0d4bfb0300                  LEAQ go.func.*+10(SB), CX
  main.go:9             0x49a8c5                48894c2448                      MOVQ CX, 0x48(SP)
  main.go:9             0x49a8ca                488d8c24f0000000                LEAQ 0xf0(SP), CX
  main.go:9             0x49a8d2                48894c2460                      MOVQ CX, 0x60(SP)
  main.go:9             0x49a8d7                48c744246801000000              MOVQ $0x1, 0x68(SP)
  main.go:9             0x49a8e0                48c744247001000000              MOVQ $0x1, 0x70(SP)
  main.go:9             0x49a8e9                488d4c2430                      LEAQ 0x30(SP), CX
  main.go:9             0x49a8ee                48890c24                        MOVQ CX, 0(SP)
  main.go:9             0x49a8f2                e89914f9ff                      CALL runtime.deferprocStack(SB)
  main.go:9             0x49a8f7                85c0                            TESTL AX, AX
  main.go:9             0x49a8f9                7562                            JNE 0x49a95d
  main.go:10            0x49a8fb                0f57c0                          XORPS X0, X0
  main.go:10            0x49a8fe                0f11842410010000                MOVUPS X0, 0x110(SP)
  main.go:10            0x49a906                488d0533270100                  LEAQ runtime.rodata+73792(SB), AX
  main.go:10            0x49a90d                4889842410010000                MOVQ AX, 0x110(SP)
  main.go:10            0x49a915                488d05840c0500                  LEAQ sync/atomic.CompareAndSwapUintptr.args_stackmap+408(SB), AX
  main.go:10            0x49a91c                4889842418010000                MOVQ AX, 0x118(SP)
  main.go:10            0x49a924                488d842410010000                LEAQ 0x110(SP), AX
  main.go:10            0x49a92c                48890424                        MOVQ AX, 0(SP)
  main.go:10            0x49a930                48c744240801000000              MOVQ $0x1, 0x8(SP)
  main.go:10            0x49a939                48c744241001000000              MOVQ $0x1, 0x10(SP)
  main.go:10            0x49a942                e8f996ffff                      CALL fmt.Println(SB)
  main.go:11            0x49a947                90                              NOPL

  main.go:11            0x49a948                e8931af9ff                      CALL runtime.deferreturn(SB)
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

