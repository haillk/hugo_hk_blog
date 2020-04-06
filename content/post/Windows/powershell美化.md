---
title: "Powershell美化"
date: 2019-12-12T21:28:50+08:00
categories:
- Windows
tags:
- powershell
- Windows
keywords:
- powershell美化
#thumbnailImage: //example.com/image.jpg
---


#### <font color=#CC3300>oh-my-posh使用和主题选择</font>

 `oh-my-posh` 是一个开源、低调的 PowerShell 主题框架。

###### 下载安装

在powershell中执行 

```powershell
Install-Module posh-git -Scope CurrentUser
Install-Module oh-my-posh -Scope CurrentUser
Import-Module posh-git 
Import-Module oh-my-posh
```

###### 选择主题

oh-my-posh包含的10个主题有：sorin,agnoster,avit,darkblood,fish,honukai,paradox,powerline,robbyrussell,tehrob

我喜欢的是sorin,比较简洁。

```powershell
set-themes sorin
```



###### 添加进配置文件

powershell中输入$profile，会输出一个文件的路径，这个文件就是shell的配置文件。

```powershell
$profile
C:\Users\Haillk\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1
```

###### 字体问题

当然是要选用伟大的黑纱字体来避免乱码问题。

#### <font color=#CC3300>颜色盘的隐含配置</font>

千万不要手贱去来回改颜色盘，系统会自动调用响应颜色的。

| 1     | 错误信息背景颜色                                  |
| ----- | ------------------------------------------------- |
| 2     | sorin中间部分颜色                                 |
| 3     | git信息                                           |
| 4     | 双引号中的字体颜色                                |
| 5     | sorin箭头颜色1                                    |
| 6     | sorin箭头颜色3                                    |
| 7     | sorin箭头颜色2                                    |
| 8     | anaconda的环境信息                                |
| 9     | --svf 类似配置颜色                                |
| 10 D7 | ls 时的其中一种文件颜色                           |
| 11 D6 | ls 时的其中一种文件颜色                           |
| 12 D5 | s 时的其中一种文件颜色                            |
| 13 D4 | 错误信息颜色                                      |
| 14 D3 |                                                   |
| 15 D2 | 命令颜色                                          |
| 16 D1 | powerline中conda的环境名称部分的颜色,文件名的颜色 |

#### <font color=#CC3300>colortool的使用</font>

1、去github上下载colortool

2、在下载下来的根目录下打开powershell `.\colortool .\ayu.itermcolors`，也可以选择其他主题

3、打开属性，点击确认  ，这步很关键，坑了我好久。T.T

4、重启

#### <font color=#CC3300>dircolor的使用</font>

不过我没用这个，备用。。

```powershell
Install-Module DirColors -Scope CurrentUser
Import-Module DirColors
Update-DirColors 配置文件的路径
```

