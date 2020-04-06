---
title: 'Golang的参数传递是传值还是传引用'
tags: 
-
categories: 
- GoLang
date: "2020-04-05T15:11:56+08:00"
comment:  true    
---

前进！前进！！不择手段的前进！

<!--more-->

### 值，指针和引用：

```c++
int i =3;//值
int* ptr = &i;//指针
int _ref=i;//引用
//只用指针和引用改变i的值
*ptr=13;
ref=13;
```

### 什么是值传递：

```go
func main() {
	i := 10
	ip := &i
	fmt.Printf("i的地址%p,ip的地址%p\n", &i, &ip)
	ff(ip)
	fmt.Printf("i的地址%p,ip的地址%p\n", &i, &ip)

}
func ff(ip *int) {
	fmt.Printf("函数内指针ip的地址%p\n", &ip)
	*ip = 1
}
//output
i的地址0xc000074090,ip的地址0xc00009e018
函数内指针ip的地址0xc00009e028
i的地址0xc000074090,ip的地址0xc00009e018
```

可以知道，函数内外的指针变量ip,虽然存的值相同，但是存在不同的内存位置，是不同的。

说明这还是值传递，只不过把指针拷贝了。

### 应用传递：

应用传递的话，函数内外的指针的地址应该是相同的。

### Map,Channel,Slice:

**Map和Channel**,**slice**中，传递的又是什么呢，虽然看起来类型，但是在参数传递时，实际上传递的还是指针的拷贝。

```go
func main() {
	persons:=make(map[string]int)
	persons["张三"]=19

	mp:=&persons

	fmt.Printf("原始map的内存地址是：%p\n",mp)
	modify(persons)
	fmt.Println("map值被修改了，新值为:",persons)
}

 func modify(p map[string]int){
	 fmt.Printf("函数里接收到map的内存地址是：%p\n",&p)
	 p["张三"]=20
 }

#output
原始map的内存地址是：0xc42000c028
函数里接收到map的内存地址是：0xc42000c038
map值被修改了，新值为: map[张三:20]
```

由于传递的是指针的拷贝，所以我们能修改map中的值。

- 使用`make`创建的map和channel，返回的是\*hmap和\*cha



