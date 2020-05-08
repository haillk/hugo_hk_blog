---
title: 'Gdb调试coredump文件'
tags: 
-
categories: 
- linux
date: "2020-05-07T20:31:29+08:00"
comment:  true    
---

佛门广大，岂不容一癫僧！

<!--more-->

### 什么是coredump文件

[参考](https://blog.csdn.net/u014403008/article/details/54174109?utm_medium=distribute.pc_relevant.none-task-blog-OPENSEARCH-3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-3)

Coredump （核心转储）。它是进程运行时在突然崩溃的一刻的内存快照。操作系统在程序发生异常而异常子啊进程内部又没有被捕获的情况下。将进程刺客的内存，寄存器状态，运行堆栈信息转储保存在一个文件中。

也是属于二进制文件，可以直接用gdb打开。

### 示例

```bash
#安装gcc
apt install build-essential

#写一个c文件
#然后编译成二进制文件
gcc -o dumpc testc.c
#运行文件,由于异常会生成coredump文件
./dumpc
#但是生成core文件需要调整系统设置。将core文件大小限制取消

```

