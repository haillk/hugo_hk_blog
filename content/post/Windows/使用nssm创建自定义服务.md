---
title: "使用nssm创建自定义服务"
date: 2019-12-17T18:58:41+08:00
lastmod: 2019-12-17T18:58:41+08:00
draft: false
description: ""
show_in_homepage: true
show_description: false
license: ''

tags: ['win10','nssm','注册自定义服务']
categories: ['Windows']

featured_image: ''
featured_image_preview: ''

comment: true
toc: true
autoCollapseToc: true
math: false
---

<!--more-->

## <font color=#CC3300>注册服务</font>

1. **使用installutil.exe**

`installutil <yourproject>.exe`

卸载`installutil /u <yourproject>.exe`

2. **使用powershell**

```powershell
New-Service -Name "YourServiceName" -BinaryPathName <yourproject>.exe
##卸载-----官方文档里是这样写的，但是我试着不管用，
Remove-Service -Name "YourServiceName"
##还是这样卸载吧
sc.exe delete "YourServiceName"
```

3. **使用sc**

```powershell
 sc create 服务名 binPath= "路径" start= auto type= share
 ##例子
 sc create Frpc binPath= "C:/frpc/windows/start.bat" start= auto type= share
```

------

 **但是有些程序在启动服务时会报错**

>  Windows无法启动 XXX 服务（位于 本地计算机上）。错误1053:服务没有及时响应启动或者控制请求
>
>  因为不是所有的exe都可以作为服务，需要使用服务封装器，比如srvany,nssm

**解决办法**：微软有一个srvany.exe可以解决此问题，不过nssm是更好的选择，这里推荐nssm

## <font color=#cc3300>安装nssm</font>

​	官网一直访问不进去，目前找到的一个可用的下载[链接](https://www.mypcrun.com/file-download-for-windows/3/8a874af5c543a7fa5a4bef61e7a1c842/692e0dfb2e973ee38a173e77e1dda958/)

​	之后就添加进path里去。

**命令列表**

```powershell
nssm install <servicename>
nssm remove <servicename>
nssm start <servicename>(可能需要管理员权限)
nssm stop <servicename>
nssm restart <servicename>
nssm edit <servicename>
```

