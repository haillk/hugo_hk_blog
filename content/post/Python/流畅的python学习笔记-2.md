---
title: "流畅的python学习笔记 2"
date: 2019-12-07T00:40:52+08:00
categories:
- 流畅的python学习笔记
tags:
- 数据结构
keywords:
- tech
#thumbnailImage: //example.com/image.jpg
---



#### <font color=#CC3300>1、列表推导</font>

列表循环：`dummy = [ord(symbol) for symbol in symbols]`

传统for循环：

```python
for symbol in symbols:
... codes.append(ord(symbol))
```

带条件的列表推导：`beyond_ascii = [ord(s) for s in symbols if ord(s) > 127] `

笛卡尔积：`tshirts = [(color, size) for color in colors for size in sizes] `

>  列表推导的作用只有一个： 生成列表。 



#### <font color=#CC3300>2、生成器表达式</font>

> 生成器表达式的语法跟列表推导差不多， 只不过把方括号换成圆括号而已 
>
> 生成器表达式背后遵守了迭代器协议， 可以逐个地产出元素， 而不是先建立一个完整的列表， 然后再把这
> 个列表传递到某个构造函数里 

初始化数组：`tuple(ord(symbol) for symbol in symbols)  `

笛卡尔积：

```python
for tshirt in ('%s %s' % (c, s) for c in colors for s in sizes): ➊
... print(tshirt)
```

#### <font color=#CC3300>3、元组tuple</font>

具名元组

```python
from collections import namedtuple
City = namedtuple('City', 'name country population coordinates') ➊
tokyo = City('Tokyo', 'JP', 36.933, (35.689722, 139.691667)) ➋

```

#### <font color=#CC3300>4、切片</font>

`seq[start:stop:step]  `

#### <font color=#CC3300>5、对序列使用+和*</font>

```python
l=[1,2,3]
print(l*3)
>>>[1,2,3,1,2,3,1,2,3]
```

