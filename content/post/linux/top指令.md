---
title: 'Top指令'
tags: 
-
categories: 
- Linux
date: "2020-04-03T15:07:11+08:00"
comment:  true    
---

*每天都有新体验

<!--more-->

### 解释一下top命令

```shell
#第一行 系统时间，运行时间，登陆的用户数量
top - 15:09:55 up 45 days,  1:43,  2 users,  load average: 0.00, 0.00, 0.00
#第二行，进程状态，
Tasks:  68 total,   2 running,  66 sleeping,   0 stopped,   0 zombie
#第三行，cpu状态5.9%us — 用户空间占用CPU的百分比。
#3.4% sy — 内核空间占用CPU的百分比。
#0.0% ni — 改变过优先级的进程占用CPU的百分比
#90.4% id — 空闲CPU百分比
#0.0% wa — IO等待占用CPU的百分比
#0.0% hi — 硬中断（Hardware IRQ）占用CPU的百分比
#0.2% si — 软中断（Software Interrupts）占用CPU的百分比
%Cpu(s):  0.7 us,  0.0 sy,  0.0 ni, 98.3 id,  0.0 wa,  0.0 hi,  0.0 si,  1.0 st
#内存状态..计算公式：第四行的free + 第四行的buffers + 第五行的cached
MiB Mem :    987.4 total,     90.1 free,    143.1 used,    754.2 buff/cache
MiB Swap:   1022.0 total,   1001.7 free,     20.2 used.    733.9 avail Mem 
#PID — 进程id
#USER — 进程所有者
#PR — 进程优先级
#NI — nice值。负值表示高优先级，正值表示低优先级
#VIRT — 进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES
#RES — 进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA
#SHR — 共享内存大小，单位kb
#S — 进程状态。D=不可中断的睡眠状态 R=运行 S=睡眠 T=跟踪/停止 Z=僵尸进程
#%CPU — 上次更新到现在的CPU时间占用百分比
#%MEM — 进程使用的物理内存百分比
#TIME+ — 进程使用的CPU时间总计，单位1/100秒
#COMMAND — 进程名称（命令名/命令行）
  PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND               
22830 root      20   0   10960   3480   3072 R   0.3   0.3   0:00.01 top                   
22831 root      20   0   16500   7596   6572 S   0.3   0.8   0:00.01 sshd                  
    1 root      20   0  170524   6768   5288 S   0.0   0.7   2:20.77 systemd               
    2 root      20   0       0      0      0 S   0.0   0.0   0:00.97 kthreadd              
    3 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_gp                
    4 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 rcu_par_gp            
    6 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 kworker/0:0H-kblockd  
    8 root       0 -20       0      0      0 I   0.0   0.0   0:00.00 mm_percpu_wq          
    9 root      20   0       0      0      0 S   0.0   0.0   4:22.78 ksoftirqd/0           
   10 root      20   0       0      0      0 R   0.0   0.0  93:31.86 rcu_sched             
   11 root      20   0       0      0      0 I   0.0   0.0   0:00.00 rcu_bh                
   12 root      rt   0       0      0      0 S   0.0   0.0   0:09.87 migration/0           
   14 root      20   0       0      0      0 S   0.0   0.0   0:00.00 cpuhp/0              
```

- 按数字1,2,3可以查看多cpu情况
- b 可以高亮显示当前进程
- x 可以高亮显示顺序的依据列，默认依据内存使用大小。
- shift + > <可以更改依据的排序列。



###    **top交互命令**

在top 命令执行过程中可以使用的一些交互命令。这些命令都是单字母的，如果在命令行中使用了s 选项， 其中一些命令可能会被屏蔽。

h 显示帮助画面，给出一些简短的命令总结说明

k 终止一个进程。

**i 忽略闲置和僵死进程。这是一个开关式命令。**

q 退出程序

r 重新安排一个进程的优先级别

S 切换到累计模式

s 改变两次刷新之间的延迟时间（单位为s），如果有小数，就换算成m s。输入0值则系统将不断刷新，默认值是5 s

f或者F 从当前显示中添加或者删除项目

o或者O 改变显示项目的顺序

l 切换显示平均负载和启动时间信息

**m 切换显示内存信息**

t 切换显示进程和CPU状态信息

c 切换显示命令名称和完整命令行

M 根据驻留内存大小进行排序

P 根据CPU使用百分比大小进行排序

T 根据时间/累计时间进行排序

​	W 将当前设置写入~/.toprc文件中 