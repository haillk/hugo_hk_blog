---
title: 'Init_call'
tags: 
-
categories: 
- Python
date: "2020-03-20T22:44:20+08:00"
comment:  true
---



\__init\__()和\__call\__()

### \__init\__()相当于类的构造器

### \__call\__()：

​	Python中的函数是一级对象，也就是说函数的引用可以作为输入传递到其他函数中去。

​	而类的实例也可以被当作函数对待，也可以作为输入传递到其他函数中取。

​	call的意义就在此：

​			当一个类实例作为函数调用时，定义call方法就对被调用。x._\_call\__(aa)等同于x(aa)

emmmmm        init返回的是实例，call返回的是函数返回值。

例子：

```python
class A:
	def __init__(self):
		print("init")
	def __call__(self):
		print("call")

a = A()
a()

##out
init
call
[Finished in 0.1s]
```

