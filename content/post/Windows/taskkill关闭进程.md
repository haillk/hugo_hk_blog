---
title: "Taskkill关闭进程"
date: 2019-12-28T01:01:55+08:00
lastmod: 2019-12-28T01:01:55+08:00
show_in_homepage: true

tags: ['windows命令行','Microsoft-doc']
categories: ['Windows']

featured_image: ''
featured_image_preview: ''

comment: true
toc: false
autoCollapseToc: true
---

<!--more-->

## taskkill /f /t /im /hugo.exe  [microsoft-doc](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/taskkill)

示例分析：`taskkill /f /t /im /hugo.exe`

/im 指定进程的映像名称

/f 强制终止程序

/t 包括子程序一起终止

> taskkill是用来终止进程的。具体的命令规则如下：
>
> TASKKILL [/S *system* [/U *username* [/P [*password*]]]]
>
> { [/FI *filter*] [/PID *processid* | /IM *imagename*] } [/F] [/T]
>
> 参数列表:
>
> /S system 指定要连接到的远程系统。
>
> /U [domain\]user 指定应该在哪个用户上下文
>
> 执行这个命令。
>
> /P [password] 为提供的用户上下文指定密码。如果忽略，提示输入。
>
> /F 指定要强行终止的进程。
>
> /FI filter 指定筛选进或筛选出查询的的任务。
>
> /PID process id 指定要终止的进程的PID。
>
> /IM image name 指定要终止的进程的映像名称。[通配符](https://baike.baidu.com/item/通配符) '*'可用来指定所有映像名。
>
> /T Tree kill: 终止指定的进程和任何由此启动的子进程。