---

title: "Python实战学习 使用代理爬取公众号文章"
date: 2019-12-17T18:09:30+08:00
lastmod: 2019-12-17T18:09:30+08:00
draft: true
description: ""
show_in_homepage: true
show_description: false
license: ''

tags: ['crawler']
categories: ['Python实战学习 使用代理爬取公众号文章']

featured_image: ''
featured_image_preview: ''

comment: true
toc: true
autoCollapseToc: true
math: false
---

<!--more-->

安装aio,requests,redis-py,pyquery,flask,pymysql这些库

## redis配置

首先从github上下载redis 的zip包[下载地址](https://github.com/MicrosoftArchive/redis/releases)

解压之后，在文件夹下中的powershell输入`redis-server.exe redis.windows.conf`即可启动服务，之后添加进path或者注册为服务即可，注册服务参考[nssm注册服务]()

安装适配包`conda install redis-py`  这里注意，导入的时候要用`import redis`而不是`mport redis-py`



测试

```python
import redis
r = redis.StrictRedis(host='localhost',port=6379,decode_responses=True)
r.set('name','haillk')
print(r['name'])

```
输出 ‘name’的值

```powershell
Haillk H:\..\EX env:hk-learn ❯❯❯ python .\test.py                           haillk
```



