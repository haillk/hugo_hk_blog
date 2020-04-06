---
title: 'Go调用shell命令'
tags: 
-
categories: 
- GoLang
date: "2020-03-11T13:23:50+08:00"
comment:  true   
---

执行shell命令，并获取返回值

```go
func execshell(){
    cmd := exec.Command("v2ray","url","**") //参数要分开
    //将返回值输出到缓存
    var out bytes.Buffer
    cmd.Stdout = &out
    //执行命令
    err:=cmd.Run()
}
```

[参考go调用shell命令](https://blog.csdn.net/qq_36874881/article/details/78234005)