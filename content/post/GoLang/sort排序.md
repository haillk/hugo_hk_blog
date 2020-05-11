---
title: 'Sort排序'
tags: 
-
categories: 
- GoLang
date: "2020-04-09T15:19:35+08:00"
comment:  true    
---

前进！前进！！不择手段的前进！

<!--more-->

### 要求：

go中的排序需要三个接口：

```go
type Interface interface {
    // Len is the number of elements in the collection.
    Len() int
    // Less reports whether the element with
    // index i should sort before the element with index j.
    Less(i, j int) bool
    // Swap swaps the elements with indexes i and j.
    Swap(i, j int)
}
```



### 基本类型排序：

```
package main
 
import (
    "fmt"
    "sort"
)
 
func main() {
    intList := [] int {2, 4, 3, 5, 7, 6, 9, 8, 1, 0}
    float8List := [] float64 {4.2, 5.9, 12.3, 10.0, 50.4, 99.9, 31.4, 27.81828, 3.14}
    stringList := [] string {"a", "c", "b", "d", "f", "i", "z", "x", "w", "y"}
   
    sort.Ints(intList)
    sort.Float64s(float8List)
    sort.Strings(stringList)
   
    fmt.Printf("%v\n%v\n%v\n", intList, float8List, stringList)
 
}
```

- 这三种方法可以对基本类型进行排序。

### 逆序：

逆序就有点麻烦了

[还是看这里的吧]([https://itimetraveler.github.io/2016/09/07/%E3%80%90Go%E8%AF%AD%E8%A8%80%E3%80%91%E5%9F%BA%E6%9C%AC%E7%B1%BB%E5%9E%8B%E6%8E%92%E5%BA%8F%E5%92%8C%20slice%20%E6%8E%92%E5%BA%8F/](https://itimetraveler.github.io/2016/09/07/[Go语言]基本类型排序和 slice 排序/))

### sort.Slice（）方法：

[参考](https://blog.csdn.net/yzf279533105/article/details/89459460)

**1. Slice() 不稳定排序**

**2. SliceStable() 稳定排序**

**3. SliceIsSorted() 判断是否已排序**

参数：`sort.Slice(interface {}, func(int, int) bool)`

```go
// 从小到大排序(稳定排序)
	sort.SliceStable(s, func(i, j int) bool {
		if s[i].value < s[j].value {
			return true
		}
		return false
	})
```

- 使用这种方法可以对结构体排序。