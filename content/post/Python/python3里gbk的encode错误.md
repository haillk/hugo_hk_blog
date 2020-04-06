---
title: "Python3里gbk的encode错误"
date: 2019-12-27T01:32:19+08:00
lastmod: 2019-12-27T01:32:19+08:00
show_in_homepage: true

tags: []
categories: ['Python']

featured_image: ''
featured_image_preview: ''

comment: true
toc: false
autoCollapseToc: true
---

<!--more-->

## 'gbk' codec can't encode character '\xa9'


先看一下出问题代码

```python
import requests
def get_one_page(url):
	headers={
		'User-Agent':'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.88 Safari/537.36'
	}
	response = requests.get(url,headers=headers)
	if response.status_code ==200:
		return response.text
	
def main():
	url = "http://maoyan.com/board/4"
	html = get_one_page(url)
	print(html)
main()
```

出问题的是第12行

错误提示

```powershell
'gbk' codec can't encode character '\xa9'
```



> 编码常识：str转byte叫做encode ,bytes转str是decode.



具体原因就是print（）函数默认编码的局限。改一下默认编码就好了。

```python
print(html.encode('utf-8'))
```

