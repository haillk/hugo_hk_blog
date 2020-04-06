---
title: "Yield完全解读"
date: 2019-12-27T02:15:34+08:00
lastmod: 2019-12-27T02:15:34+08:00
show_in_homepage: true

tags: ['Yield']
categories: ['Python']

featured_image: ''
featured_image_preview: ''

comment: true
toc: true
autoCollapseToc: true
---

<!--more-->

## 迭代   [参考-较详细](https://liam.page/2017/06/30/understanding-yield-in-python/)

在实际使用list,tuple等容器时，容器中会保存容器中所有的数据。

以list为例，如果list在内存中加载全部的数据会造成大量的 消耗，但是我们并不会同时使用全部的数据，这就需要一种更聪明的方法，类似sql底层的B+树，随取随用。

这就是迭代要实现的功能： **逐个获取元素**

## 迭代器

我们常用的list,tuple等都实现了迭代，我们自定义的类只要实现了\__iter\__()方法也是可以迭代的。

迭代器和for循环的联系：

​	Python 中的迭代器协议和 Python 中的 `for` 循环是紧密相连的。

```python
for x in something:  
	print(x)
```

Python 处理 `for` 循环时，首先会调用内建函数 `iter(something)`，它实际上会调用 `something.__iter__()`，返回 `something` 对应的迭代器。而后，`for` 循环会调用内建函数 `next()`，作用在迭代器上，获取迭代器的下一个元素，并赋值给 `x`。此后，Python 才开始执行循环体。

## 生成器

生成器是特殊的迭代器。

## yield表达式

[廖雪峰-利用斐波那契函数举例](https://www.ibm.com/developerworks/cn/opensource/os-cn-python-yield/index.html)

> yield 的作用就是把一个函数变成一个 generator，带有 yield 的函数不再是一个普通函数，Python 解释器会将其视为一个 generator，调用 fab(5) 不会执行 fab 函数，而是返回一个 iterable 对象！在 for 循环执行时，每次循环都会执行 fab 函数内部的代码，执行到 yield b 时，fab 函数就返回一个迭代值，下次迭代时，代码从 yield b 的下一条语句继续执行，而函数的本地变量看起来和上次中断执行前是完全一样的，于是函数继续执行，直到再次遇到 yield。

实例：

```python
def square():
    for x in range(4):
        yield x ** 2
square_gen = square()
for x in square_gen:
    print(x)
```

前面说到，`for` 循环会调用 `iter()` 函数，获取一个生成器；而后调用 `next()` 函数，将生成器中的下一个值赋值给 `x`；再执行循环体。因此，上述 `for` 循环基本等价于：

```python
genitor = square_gen.__iter__()
while True:
    x = geniter.next() # Python 3 是 __next__()
    print(x)
```

[参考-简书](https://www.jianshu.com/p/d09778f4e055)

