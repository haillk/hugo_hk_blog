---
title: 'TCp中的nagle算法'
tags: 
- TCP
categories: 
- 网络
date: "2020-04-06T14:21:02+08:00"
comment:  true    
---

前进！前进！！不择手段的前进！

<!--more-->

由于在有些使用TCp的场景中，会发送少量的数据字节，这量就会产生20个字节的IP,header和20个字节的TCPheader.

加少量的数据字节，造成的巨额开销。会使这种small packet在广域网上增加拥塞。

**Nagle算法**就是一种通过减少需要通过网络发送包的数量来提高TCP/ip传输效率的算法。

### Nagle算法:

**基本定义：**在任意时刻，最多只能有一个未被确认的小段。有多个包，当小包1发送了，但是未接到ack之前，其他小包就先等待，并和其他小包整合为一个打包，直到第一个小包的ack回来，再把剩余整合到一起的包一起发送。

**禁用nagle**:设置TCP_NODELAY参数

### [Delay ACK和 NAgle](https://blog.csdn.net/wdscq1234/article/details/52432095?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-3&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-3):

> Nagle指出Nagle算法与Delay ACK机制有共存的情况下会有一些非常糟糕的状况，比如举一个场景：PC1和PC2进行通信，PC1发数据给PC2，PC1使用Nagle算法，PC2有delay ACK机制
>
> 1. PC1发送一个数据包给PC2，PC2会先不回应，delay ACK
>
> 2. PC1再次调用send函数发送小于MSS的数据，这些数据会被保存到Buffer中，等待ACK，才能再次被发送
>
> 从上面的描述看，显然已经死锁了，PC1在等待ACK，PC2在delay ACK，那么解锁的代价就是Delay ACK的Timer到期，至少40ms[40ms~500ms不等]，也就是2种算法在通信的时候，会产生不必要的延时！
> 