---
title: "流畅的python学习笔记 1"
date: 2019-12-06T01:43:47+08:00
categories:
- 流畅的python学习笔记
tags:
- 特殊方法
keywords:
- python特殊方法
#thumbnailImage: //example.com/image.jpg  
---



#### <font color=#CC3300>特殊方法的格式</font>

> 特殊方法的名字以两个下划线开头,两个下划线结尾(例如\__getitem\__)。比如 obj[key]
> 的背后就是 __getitem__ 方法， 为了能求得 my_collection[key] 的值， 解释器实际上会调用 my_collection.__getitem__(key)。 

> 魔术方法（magic method） 是特殊方法的昵称 ,特殊方法也叫双下方法（dunder method）

#### <font color=#CC3300>一个二维向量加法的例子</font>

​	构建一个Vector类,实现加法，即：Vector(1,2)+Vector(1,2)=Vector(2,4),下面代码实现了更多特殊方法。

```python
from math import hypot
class Vector:
	def __init__(self, x=0, y=0):
		self.x = x
		self.y = y
	def __repr__(self):
		return 'Vector(%r, %r)' % (self.x, self.y)
	def __abs__(self):
		return hypot(self.x, self.y)
	def __bool__(self):
		return bool(abs(self))
	def __add__(self, other):
		x = self.x + other.x
		y = self.y + other.y
		return Vector(x, y)
	def __mul__(self, scalar):
		return Vector(self.x * scalar, self.y * scalar)
```
​	下面是调用结果,repr特殊方法规定了字符串的表达形式。

#### <font color=#CC3300>总结</font>

个人理解：特殊方法是系统会频繁调用的中间方法。用户使用时不会直接调用，但是在调用一些方法时，会间接调用。用户调用普通方法--->系统调用特殊方法完成目标。更改特殊方法，就能更改一些普通方法的底层调用方法。

> 通过实现特殊方法， 自定义数据类型可以表现得跟内置类型一样， 从而让我们写出更具表达力的代码 
