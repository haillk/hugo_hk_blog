---
title: 'With As'
tags: 
- 
categories: 
- Python
date: "2020-03-10T00:09:42+08:00"
comment:  true  
---

 with语句，对于实现需要设置，事后做清理工作，with语句就能很好的处理：

在不用with语句：

```python
file = open("/temp")
try:
	data = file.read()
finally:
    file.close()
```

使用with语句：

```python
with open("/tmp/fs") as file:
    data = file.read()
```

### with如何工作：

​	with所打开的对象必须有一个enter（）方法个exit()方法。所用共有两个阶段

​	1、紧跟这with的语句被求值后，对象的enter（）方法被调用，

​     2、with中的代码被执行结束后，调用兑现的exit()方法

这两个方法中也可以放自定义的清理资源，关闭文件等操作。

[参考Python中的with as 用法](https://www.jianshu.com/p/c00df845323c)