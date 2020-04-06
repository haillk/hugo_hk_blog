---
title: "双冒号和zip函数"
date: 2020-02-17T20:05:10+08:00
lastmod: 2020-02-17T20:05:10+08:00
show_in_homepage: true

tags: ['Python基础']
categories: ['Python']

featured_image: ''
featured_image_preview: ''

comment: true
toc: false
autoCollapseToc: true
---

<!--more-->

### 双冒号表示有间隔的取列表中的值：

示例：

```python
a = [1,2,3,4,5,6]
print(a[::2])
#############
#output
[1,3,5]
```

### zip函数，返回两个列表中相对应元素组成的元组的列表对象。注意直接返回的是对象，用for in range可以遍历输出。 

```python
a = [1,2,3]
b = [4,5,6]
list(zip(a,b))
#############
output
[(1,4),(2,5),(3,6)]
```

