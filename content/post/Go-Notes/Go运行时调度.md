---
title: 'Go运行时调度'
tags: 
-
categories: 
- 
date: "2020-03-31T15:39:51+08:00"
comment:  true    
---

*每天都有新体验

<!--more-->

### 要求

对与Go中协程的要求要在这些需求之内

1. 应该是并行、可扩展且公平的。
2. 每个进程应可扩展到数百万个goroutine（[C10M](http://c10m.robertgraham.com/p/manifesto.html)）
3. 内存利用率高。（RAM很便宜，但不是免费的。）
4. 系统调用不应导致性能下降。（最大化吞吐量，最小化等待时间）

### goroutine结构

GO的做法是将N个goroutines映射到M个内核线程上去。Goroutines是go代码，因此我们可以完全控制他，他在用户空间汇总，创建起来很便宜。

并且goroute也有相应的状态，来帮助调度器很据goroutines状态知道该调用那一个goroutines.

与内核线程的状态信息相比，goroutine的状态信息很小，因此goroutine的上下问切换很快。

goroutine的状态有（running,runnable,blocked）

### M:G结构：

 在简单的M:N调度器中，有一个全局运行队列。某些操作讲一个新的goroutine放入队列。

每个M线程通过互斥锁来同步对该运行队列的访问。

### 阻塞的goroutine在什么位置呢：

goroutine可能会阻塞的情况：

1. 在channel上发送和接收
2. 网络IO
3. 阻塞的系统调用
4. 使用定时器
5. 使用互斥锁

#### channel操作期间阻塞的goroutine：

每个channel都有一个**recvq(waitq)**，用于存储试图从该channel读取数据而阻塞的goroutine。

**Sendq(waitq)**存储试图将数据发送到channel而被阻止的goroutine 。（channel实现原理:-https://codeburst.io/diving-deep-into-the-golang-channels-549fd4ed21a8）

channel本身会将channel操作后未阻塞的goroutine的放入运行队列（runqueue）

Sendq(waitq)存储视图将数据发送到channel而被阻止

#### 系统调用：

系统调用会阻塞底层内核线程，因此无法再在阻塞的M线程上安排其他的Goroutine。

但是有超额的goroutine未完成，我们将它放在运行队列中（runqueue）.而不是返回调用。

### P:处理器，可以讲其视为在线程上运行的本地调度程序。

> 逻辑进程P的数量始终是固定的。（默认为当前进程可以使用的逻辑cpu数量）
>
> 然后将本地运行队列放入固定数量的逻辑处理器（P）中。

### 使用固定逻辑处理器的系统调用。

> go通过将他们包装在运行时中来优化系统调用。从字面上看，某些逻辑在进入系统调用之前被执行，而某些逻辑在系统调用返回之后执行。**进行阻塞的系统调用时**，此包装器将自动将P与线程M(即将执行阻塞系统调用的线程)解绑，并允许另一个线程在其上运行

<img src="https://pic.downk.cc/item/5e858906504f4bcb04d11cba.jpg" style="zoom:50%;" />

### 流程：

（1）创建一个**goroutine**，它会放在**全局运行队列**中，等待调度器调度

（2）**调度器**将这个goroutine 分配给一个**逻辑处理器A**，将它放到了这个逻辑处理器的**本地队列**中，这个goroutine就会**等待**逻辑处理器A**执行**它

（3）每个逻辑处理器默认**绑定了一个线程**，它是在线程中去**执行自己本地队列**中的goroutine。

- 如果逻辑处理器目前运行的goroutine是阻塞的，比如打开文件操作。

（4）逻辑处理器和原来的**线程分离**，调度器重新**创建一个线程**和这个逻辑处理器**绑定**。这时候逻辑处理器在新的线程上继续执行本地运行队列的其他goroutine。 同时，阻塞的goroutine随着线程分离，从本地队列移除。

（5）那个阻塞的goroutine和分离的线程会继续阻塞，等待系统调用的返回。一旦执行完成并返回，这个goroutine就会**重新放回**到原来逻辑处理器的本地队列。

（6）之前的线程目前没有goroutine了，但是它会被保存，以备之后使用。

**Note**:第（4）（5）步，如果goroutine 是在执行网络io的操作，这个goroutine就不一定就回到这个逻辑处理器了。它实际上会先从逻辑处理器分离，移到集成了网络轮询器的运行时 ，一旦该轮询器指示某个网络读或者写操作已经就绪，对应的 goroutine 就会**重新分配**到逻辑处理器上来完成操作 。

### 并发和并行：

go语言实现并发，创建多个goroutine，调度器会将goroutine分配到逻辑处理器的本地运行队列，逻辑处理器去运行goroutine。如果只有一个逻辑处理器，只会实现并发，不会实现并行。

要实现并行，就需要多个逻辑处理器，在不同的cpu上，然后调度器会平等的将goroutine分配到每个逻辑处理器，这样多个线程多个goroutine就实现了并行和并发。 至于这些算法怎么调度，我们根本不需要关心，我们只要记住goroutine是我们进行并发编程的一个独立单元就可以了。