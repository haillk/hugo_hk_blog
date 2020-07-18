---
title: "跟着无闻学Go Web应用开发 进阶模板"
date: 2019-12-19T16:55:01+08:00
lastmod: 2019-12-19T16:55:01+08:00
description: ""
show_in_homepage: true
show_description: false
license: ''

tags: ['go','web']
categories: ['跟着无闻学Go-web应用开发']

featured_image: ''
featured_image_preview: ''

comment: true
toc: true
autoCollapseToc: true
math: false
---

 在Go 语言提供的模板引擎中进行条件判断和更加复杂的逻辑操作

<!--more-->

## <font color=#cc3300>在模板中定义变量</font>

这里变量得格式很好理解，在{{ }}中的用$开头的变量即可，例如{{ $name }},并且使用:=赋值。

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"text/template"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		// 创建模板对象并解析模板内容
		tmpl, err := template.New("test").Parse(`
{{$name := "Alice"}}
{{$age := 18}}
{{$round2 := true}}
Name: {{$name}}
Age: {{$age}}
Round2: {{$round2}}
`)
		if err != nil {
			fmt.Fprintf(w, "Parse: %v", err)
			return
		}

		// 调用模板对象的渲染方法
		err = tmpl.Execute(w, nil)
		if err != nil {
			fmt.Fprintf(w, "Execute: %v", err)
			return
		}
	})

	log.Println("Starting HTTP server...")
	log.Fatal(http.ListenAndServe("localhost:4000", nil))
}
```

## <font color=#cc3300>在模板中使用if语句</font>

看下代码就能很好的明白了

```go
package main

import (
	...
	"strconv"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		// 创建模板对象并解析模板内容
		tmpl, err := template.New("test").Parse(`
{{if .yIsZero}}
	除数不能为 0
{{else}}
	{{.result}}
{{end}}
`)
		if err != nil {
			fmt.Fprintf(w, "Parse: %v", err)
			return
		}

		// 获取 URL 查询参数的值
		// 注意：为了简化代码逻辑，这里并没有进行错误处理
		x, _ := strconv.ParseInt(r.URL.Query().Get("x"), 10, 64)
		y, _ := strconv.ParseInt(r.URL.Query().Get("y"), 10, 64)

		// 当 y 不为 0 时进行除法运算
		yIsZero := y == 0
		result := 0.0
		if !yIsZero {
			result = float64(x) / float64(y)
		}

		// 调用模板对象的渲染方法
		err = tmpl.Execute(w, map[string]interface{}{
			"yIsZero": yIsZero,
			"result":  result,
		})
		if err != nil {
			fmt.Fprintf(w, "Execute: %v", err)
			return
		}
	})

	...
}
```

## <font color=#cc3300>在模板中的等式和不等式</font>

用于等式与不等式判断的函数主要有以下六种（均接受两个，分别名为 `arg1` 和 `arg2` 的参数）：

- `eq`：当等式 `arg1 == arg2` 成立时，返回 true，否则返回 false
- `ne`：当不等式 `arg1 != arg2` 成立时，返回 true，否则返回 false
- `lt`：当不等式 `arg1 < arg2` 成立时，返回 true，否则返回 false
- `le`：当不等式 `arg1 <= arg2` 成立时，返回 true，否则返回 false
- `gt`：当不等式 `arg1 > arg2` 成立时，返回 true，否则返回 false
- `ge`：当不等式 `arg1 >= arg2` 成立时，返回 true，否则返回 false

如果你对这些函数的名字感到奇怪，其实不难发现这些名字本质上就是相关英文的缩写。如 “eq” 是 “equal” 的缩写，”ne” 表示 “not equal”，”lt” 表示 “less than“，”le” 表示 “less than or equal” 等等。

代码演示

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"text/template"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		// 创建模板对象并解析模板内容
		tmpl, err := template.New("test").Parse(`
{{$name1 := "alice"}}
{{$name2 := "bob"}}
{{$age1 := 18}}
{{$age2 := 23}}

{{if eq $age1 $age2}}
	年龄相同
{{else}}
	年龄不相同
{{end}}

{{if ne $name1 $name2}}
	名字不相同
{{end}}

{{if gt $age1 $age2}}
	alice 年龄比较大
{{else}}
	bob 年龄比较大
{{end}}
`)
		if err != nil {
			fmt.Fprintf(w, "Parse: %v", err)
			return
		}

		// 调用模板对象的渲染方法
		err = tmpl.Execute(w, nil)
		if err != nil {
			fmt.Fprintf(w, "Execute: %v", err)
			return
		}
	})

	log.Println("Starting HTTP server...")
	log.Fatal(http.ListenAndServe("localhost:4000", nil))
}
```

## <font color=#cc3300>在模板中使用迭代操作</font>

go语言标准库提供的模板引擎还支持通过range语句进行迭代操作。常见的可以迭代的类型，数组，，切片，map

代码演示

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"text/template"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		// 创建模板对象并解析模板内容
		tmpl, err := template.New("test").Parse(`
{{range $name := .Names}}
	{{$name}}
{{end}}
`)
		if err != nil {
			fmt.Fprintf(w, "Parse: %v", err)
			return
		}

		// 调用模板对象的渲染方法
		err = tmpl.Execute(w, map[string]interface{}{
			"Names": []string{
				"Alice",
				"Bob",
				"Carol",
				"David",
			},
		})
		if err != nil {
			fmt.Fprintf(w, "Execute: %v", err)
			return
		}
	})

	log.Println("Starting HTTP server...")
	log.Fatal(http.ListenAndServe("localhost:4000", nil))
}
```

## <font color=#cc3300>在模板中使用语境操作（with 语句）</font>

先看一下实例

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"text/template"
)

type Inventory struct {
	SKU       string
	Name      string
	UnitPrice float64
	Quantity  int64
}

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		// 创建模板对象并解析模板内容
		tmpl, err := template.New("test").Parse(`Inventory
SKU: {{.Inventory.SKU}}
Name: {{.Inventory.Name}}
UnitPrice: {{.Inventory.UnitPrice}}
Quantity: {{.Inventory.Quantity}}
`)
		if err != nil {
			fmt.Fprintf(w, "Parse: %v", err)
			return
		}

		// 调用模板对象的渲染方法
		err = tmpl.Execute(w, map[string]interface{}{
			"Inventory": Inventory{
				SKU:       "11000",
				Name:      "Phone",
				UnitPrice: 699.99,
				Quantity:  666,
			},
		})
		if err != nil {
			fmt.Fprintf(w, "Execute: %v", err)
			return
		}
	})

	log.Println("Starting HTTP server...")
	log.Fatal(http.ListenAndServe("localhost:4000", nil))
}
```

这里为了渲染“inventory”的每一个值，需要一个一个用.操作来获取值。

而with语句就是为了简化这种语句的。

```go
func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		// 创建模板对象并解析模板内容
		tmpl, err := template.New("test").Parse(`Inventory
{{with .Inventory}}
	SKU: {{.SKU}}
	Name: {{.Name}}
	UnitPrice: {{.UnitPrice}}
	Quantity: {{.Quantity}}
{{end}}
`)

```

## <font color=#cc3300>在模板中空白符号处理</font>

在编写内容的时候，为了个格式清晰，可能会加上额外的空行，而我们又不想让这些空行显示出来，go也提供了剔除空行的方法。

Go 语言标准库模板引擎的一个特殊语法，`{{- `和 `-}}`。

`{{- `表示剔除模板内容左侧的所有空白符号，` -}}` 表示剔除模板内容右侧的所有空白符号

```go
...

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		// 创建模板对象并解析模板内容
		tmpl, err := template.New("test").Parse(`Inventory
{{- with .Inventory}}
	SKU: {{.SKU}}
	Name: {{.Name}}
	UnitPrice: {{.UnitPrice}}
	Quantity: {{.Quantity}}
{{- end}}
`)

...
```

