---
title: 'Gdb学习'
tags: 
-
categories: 
- Linux
date: "2020-04-09T14:39:46+08:00"
comment:  true    
---

前进！前进！！不择手段的前进！

<!--more-->

TUI界面，

`gdb -tui test`

`gdb -q test`不打印gdb版本信息。

list l 查看源码

b 23 设置断点

info breakpoints 显示断点信息

s 单步操作，进入函数，fin  退出函数

n 单步操作，不进入函数

print 查看变量

until 推出循环

bt