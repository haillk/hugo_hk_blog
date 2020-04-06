---
title: "跟着无闻学Go Web应用开发 高级模板"
date: 2019-12-20T16:55:01+08:00
lastmod: 2019-12-20T16:55:01+08:00
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

上一节学习了模板引擎的逻辑控制和空白符号处理用法。这一节学习一下，高级概念和用法，并将讯安然结果转换为html。

<!--more-->

## <font color=#cc3300>模板中的作用域</font>

之前提到的with语句就有作用域的概念，先看一个例子。

```go
tmpl, err := template.New("test").Parse(`
{{$name1 := "alice"}}
name1: {{$name1}}
    {{with true}}
        {{$name1 = "alice2"}}
        {{$name2 := "bob"}}
        name2: {{$name2}}
    {{end}}
name1 after with: {{$name1}}
`)
```

输出结果

```go
➜ curl http://localhost:4000
name1: alice
name2: bob
name1 after with: alice2
```

**结果分析：with代码块中的变量修改（代码块中未定义该变量）可以影响with代码块之外的作用域。**

在看一个例子

```go
..
		tmpl, err := template.New("test").Parse(`
{{$name1 := "alice"}}
name1: {{$name1}}
{{with true}}
	{{$name1 = "alice2"}}
	{{$name2 := "bob"}}
	name2: {{$name2}}
{{end}}
name1 after with: {{$name1}}
//额外添加这一句
name2 after with: {{$name2}}
`)
...
```

输出结果：

```go
➜ curl http://localhost:4000
Parse: template: test:10: undefined variable "$name2"
```

**结果分析：然而with语句块中定义的变量不能在with语句块外访问。**

最后一个例子：

```go
...
		tmpl, err := template.New("test").Parse(`
{{$name1 := "alice"}}
name1: {{$name1}}
{{with true}}
	{{$name1 := "alice2"}}
	{{$name2 := "bob"}}
	name1 in with: {{$name1}}
	name2: {{$name2}}
{{end}}
name1 after with: {{$name1}}
`)
...
```

输出结果：

```
➜ curl http://localhost:4000
name1: alice
name1 in with: alice2
name2: bob
name1 after with: alice
```

**结果分析：当我们在模板中使用 `:=` 的时候，模板引擎会在当前作用域内新建一个同名的模板变量（等同于程序代码中本地变量和全局变量的区别），在同个作用域内对这个模板变量的操作都不会影响到其它作用域。**

## <font color=#cc3300>模板函数</font>

模板函数，顾名思义，就是像在程序代码中的函数那样，用于在运行时调用的数据结构。

如果想要自定义模板函数并加入到模板对象中，可以通过 `Funcs` 方法：

实例：

```go
func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		// 创建模板对象并添加自定义模板函数
		tmpl := template.New("test").Funcs(template.FuncMap{
			"add": func(a, b int) int {
				return a + b
			},
		})

		// 解析模板内容
		_, err := tmpl.Parse(`
result: {{add 1 2}}
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

**分析：通过在模板上调用funcs方法添加一个funcmap，之后在解析时就能调用设定好的方法。**

## <font color=#cc3300>模板中的管道操作</font>

和linux中的管道符概念类似，先看代码：

```go
...
		tmpl := template.New("test").Funcs(template.FuncMap{
			"add2": func(a int) int {
				return a + 2
			},
		})

		// 解析模板内容
		_, err := tmpl.Parse(`
result: {{add2 0 | add2 | add2}}
`)
...
```

输出结果：

```go
➜ curl http://localhost:4000
result: 6
```

这点也很好理解。

## <font color=#cc3300>模板复用</font>

先看代码：

```go
http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		// 创建模板对象并添加自定义模板函数
		tmpl := template.New("test").Funcs(template.FuncMap{
			"join": strings.Join,
		})

		// 解析模板内容
		_, err := tmpl.Parse(`
{{define "list"}}
    {{join . ", "}}
{{end}}
Names: {{template "list" .names}}
`)
		if err != nil {
			fmt.Fprintf(w, "Parse: %v", err)
			return
		}

		// 调用模板对象的渲染方法
		err = tmpl.Execute(w, map[string]interface{}{
			"names": []string{"Alice", "Bob", "Cindy", "David"},
		})
		if err != nil {
			fmt.Fprintf(w, "Execute: %v", err)
			return
		}
	})
```

1. 首先为模板添加一个名为join函数,实际上是调用strings.join

2. 在解析时，{{define "list"}}定义了一个非常简单的局部模板，即以跟对象.作为参数调用join函数
3. `Names:{{template "list" .names}}  `  通过`template "<名称> <参数>"`的语法，调用list局部模板，并将names作为参数传递，传递的参数会成为局部模板的跟对象。

输出结果

```go
➜ curl http://localhost:4000
Names: Alice, Bob, Cindy, David
```

## <font color=#cc3300>从本地文件加载模板</font>

代码实例：

```go
package main

import (
	"fmt"
	"log"
	"net/http"
	"text/template"
)

func main() {
	// 创建模板对象并解析模板内容
	tmpl, err := template.ParseFiles("template_local.tmpl")
	if err != nil {
		log.Fatalf("Parse: %v", err)
	}

	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		// 调用模板对象的渲染方法
		err = tmpl.Execute(w, map[string]interface{}{
			"names": []string{"Alice", "Bob", "Cindy", "David"},
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

`template.ParseFiles("template_local.tmpl")`即是加载文件的方法。

我们在同个目录创建一个名为 `template_local.tmpl` 的模板文件（文件后缀可以是任意的，一般在使用标准库的模板引擎时习惯性地将文件后缀命名为 `.tmpl` 或 `.tpl`）：

```
{{range .names}}    - {{.}}{{end}}
```

尝试运行以上代码可以在终端获得以下结果：

```
➜ curl http://localhost:4000- Alice- Bob- Cindy- David
```

而且`template.ParseFiles("template_local.tmpl")`函数可以接受多个参数，并且通过使用`ExecuteTemplate`方法，来选择模板渲染。

实例：

```go
...
		// 渲染指定模板的内容
		err = tmpl.ExecuteTemplate(w, "template_local.tmpl", map[string]interface{}{
			"names": []string{"Alice", "Bob", "Cindy", "David"},
		})
...
```

## <font color=#cc3300>`html/template` 与 `text/template` 的关联与不同</font>

使用`text/template`包输出html

实例代码：

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
<html>
<body>
	<h2>Heading 2</h2>
	<p>Paragraph</p>
</boyd>
</html>
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

**结果分析：然而就是在文本里写html格式的代码，emmmmmm这不是我想要的结果。**

标准库的另一个包`http/template`,是对`text/template`的一层封装。emm具体看一下吧。

```go
···
tmpl, err := template.New("test").Parse(`
<html>
<body>
	<p>{{.content}}</p>
</boyd>
</html>
`)
		if err != nil {
			fmt.Fprintf(w, "Parse: %v", err)
			return
		}

		// 调用模板对象的渲染方法
		err = tmpl.Execute(w, map[string]interface{}{
			"content": "<script>alert('you have been pwned')</script>",
		})
···
```

>
>
>有一定 Web 开发基础的同学肯定马上就能看出来，如果我们运行这段代码，将会导致俗称的跨站脚本攻击（Cross-site scripting, XSS），是最常见的 Web 应用安全漏洞之一。
>
>如果想要避免此类攻击，只需要将导入的包从 `text/template` 改成 `html/template` 就可以了。修改完成后，再运行程序的话，我们只会看到被转义之后的 JavaScript 脚本内容，成功地避免了此类安全漏洞。

**结论：http/template提供安全保障和对文本进行了转义处理。**

## <font color=#cc3300>反转义</font>

`http/template`可以为我们对可以内容进行转义，但是有些时候确实需要动态生成html。这时候，可以借助模板函数，将文本转换为一个特殊类型template.Html

实例：

```go
package main

import (
	"fmt"
	"html/template"
	"log"
	"net/http"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		// 创建模板对象并添加自定义模板函数
		tmpl := template.New("test").Funcs(template.FuncMap{
			"safe": func(s string) template.HTML {
				return template.HTML(s)
			},
		})

		// 解析模板内容
		_, err := tmpl.Parse(`
<html>
<body>
	<p>{{.content | safe}}</p>
</boyd>
</html>
`)
		if err != nil {
			fmt.Fprintf(w, "Parse: %v", err)
			return
		}

		// 调用模板对象的渲染方法
		err = tmpl.Execute(w, map[string]interface{}{
			"content": "<b>Hello world!</b>",
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

当然也有第三方的包，可以帮助我们在渲染html时过滤调不安全内容，而非无脑的转义

例如（"github.com/microcosm-cc/bluemonday）

## <font color=#cc3300>修改分隔符</font>

我们来快速学习一下如何修改模板的分隔符，因为标准库的模板引擎使用的花括号 `{{` 和 `}}` 和许多流行的前端框架有冲突（如 VueJS 和 AngularJS），所以知道怎么修改它们是非常有用的。

```go
tmpl, err := template.New("test").Delims("[[", "]]").Parse(`[[.content]]`)
```

我们通过 `Delims` 方法将它们分别修改为方括号 `[[` 和 `]]`。