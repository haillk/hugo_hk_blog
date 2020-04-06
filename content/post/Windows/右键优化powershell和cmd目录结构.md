---
title: "右键菜单优化powershell和cmd目录结构"
date: 2019-12-21T23:35:47+08:00
lastmod: 2019-12-21T23:35:47+08:00
show_in_homepage: true

tags: ['注册表','右键菜单','powershell']
categories: ['Windows']

featured_image: ''
featured_image_preview: ''

comment: true
toc: false
autoCollapseToc: true  
---

实现在空白处直接右键打开powshell和cmd ----------和shift+右键打开管理员权限

<!--more-->

### <font color=#cc3300>实现效果</font>

- 右键

>在此处打开 Cmd窗口
>
>在此处打开powershell窗口

- shift+右键

> 在此处打开Cmd窗口(admin)
>
> 在此处打开powershell窗口(admin)

### <font color=#cc3300>打开注册表编辑器</font>

编辑右键菜单肯定是要用注册表编辑器

​	**win+R 打开运行-->regedit打开注册表编辑器**

在该目录下编辑注册表

```powershell
计算机\HKEY_CLASSES_ROOT\Directory\Background\shell
```

### <font color=#cc3300>右键菜单添加cmd和powershell</font>

#### 添加cmd

```powershell
[HKEY_CLASSES_ROOT\Directory\Background\shell\OpenCmdHere]
@="在此处打开命令行窗口"
 
[HKEY_CLASSES_ROOT\Directory\Background\shell\OpenCmdHere\command]
@="cmd.exe -noexit -command Set-Location -literalPath \"%V\"" 
```

> 上述代码的含义是在响应目录下创建项，并设置默认值

#### 移动powershell到右键菜单

​	**删除`计算机\HKEY_CLASSES_ROOT\Directory\Background\shell\Powershell`下的Extended字符串值**

> Extended 就是控制是否在右键还是在shift+右键下的。

**没有权限不要紧，文后有获取权限的方法。**

### <font color=#cc3300>Shift+右键菜单添加管理员权限的cmd和powershell</font>

#### 添加cmd(admin)

```powershell
[HKEY_CLASSES_ROOT\Directory\Background\shell\runas]
@="在此处打开命令行窗口(管理员)"
"ShowBasedOnVelocityId"=dword:00639bc8
 "Extended"=""
[HKEY_CLASSES_ROOT\Directory\Background\shell\runas\command]
@="cmd.exe /s /k pushd \"%V\""
```

#### 添加powershell(admin)  

```powershell
[HKEY_CLASSES_ROOT\Directory\Background\shell\PowershellAdmin]
@="在此处打开 Powershell 窗口(管理员)"
"Extended"=""
 
[HKEY_CLASSES_ROOT\Directory\Background\shell\PowershellAdmin\command]
@="\"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe\" -windowstyle hidden -Command $stpath = pwd; Start-Process PowerShell -ArgumentList \\\"-NoExit\\\", \\\"-Command Set-Location -literalPath '%V'\\\" -verb RunAs"
 
```

### <font color=#cc3300>获取权限的方法</font>

参考 [scdn链接](https://blog.csdn.net/yongshi6/article/details/50607493)

1. 在项上右键----->选择权限----->高级
2. 更改所有者----->设置为管理员或者自己账号（并对目录下文件都生效）
3. 为自己账号添加完全控制权限
4. ok