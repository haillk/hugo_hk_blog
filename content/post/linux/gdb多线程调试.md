---
title: 'Gdb多线程调试'
tags: 
-
categories: 
- linux
date: "2020-05-07T18:51:01+08:00"
comment:  true    
---

前进！前进！！不择手段的前进！

<!--more-->

### 显示进程

**info threads**

```shell
(gdb) info threads
  Id   Target Id         Frame
* 1    LWP 32630 "test"  main.f0 (ch=0xc000042070) at /root/gowork/src/test_gdb/test.go:7
  2    LWP 32634 "test"  runtime.usleep () at /usr/local/go/src/runtime/sys_linux_amd64.s:146
  3    LWP 32635 "test"  runtime.futex () at /usr/local/go/src/runtime/sys_linux_amd64.s:568
  4    LWP 32636 "test"  runtime.futex () at /usr/local/go/src/runtime/sys_linux_amd64.s:568

```

### 切换线程

```
thread ID
```

### 线程锁

```bash
set scheduler-locking off|on
```

> **off**:不锁定任何线程，当程序继续运行的时候如果有断点，那么就把所有的线程都暂停下来，直到你指定某个线程继续执行，如果在当前线程下使用continue的话那么会启动所有线程（GDB默认）；
> **on**:打开线程锁，锁定其他线程，只有当前线程执行。

### non-stop模式

> 当某个或多个线程在一个断点上，其他线程仍会并行运行
> \2. 你可以选择某个被中断的线程，只让他运行。

```bah
set target-async 1
set pagination off
set non-stop on
```

