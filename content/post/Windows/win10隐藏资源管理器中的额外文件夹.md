---
title: "Win10隐藏资源管理器中的额外文件夹"
date: 2019-12-13T23:57:23+08:00
categories:
- Windows
tags:
- 管理器中的额外文件夹
- regedit
keywords:
- Win10隐藏资源管理器中的额外文件夹
#thumbnailImage: //example.com/image.jpg
---

#### 打开注册表编辑器

方法：【win+R】打开运行框，输入“regedit”进入

#### 目标文件夹

该路径下由很多文件夹，找到相应的文件夹。

------

`HKEY_LOCAL_MACHINE＼SOFTWARE＼Microsoft＼Windows＼CurrentVersion＼Explorer＼FolderDescriptions`

```
图片文件夹：{0ddd015d-b06c-45d5-8c4c-f59713854639}＼PropertyBag

视频文件夹：{35286a68-3c57-41a1-bbb1-0eae73d76c95}＼PropertyBag

下载文件夹：{7d83ee9b-2244-4e70-b1f5-5393042af1e4}＼PropertyBag

音乐文件夹：{a0c69a99-21c8-4671-8703-7934162fcf1d}＼PropertyBag

桌面文件夹：{B4BFCC3A-DB2C-424C-B029-7FE99A87C641}＼PropertyBag

文档文件夹：{f42ee2d3-909f-4907-8871-4c22fc0bf756}＼PropertyBag
```

#### 修改值

接着展开进入它的子项“PropertyBag”，双击右侧窗口中的“ThisPCPolicy”，将其数据数值由Show改为Hide，最后确定即可。

然后刷新资源管理器，即可实现目标。

#### 删除3D对象的文件夹

定位到

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace\`

删除`{0DB7E03F-FC29-4DC6-9020-FF41B59E513A}`

#### 遗留问题

一些第三方的32位应用程序在浏览文件夹时仍然可能会显示这六个文件夹，需要到一下位置，再执行一遍相同的操作即可。

HKEY_LOCAL_MACHINE\SOFTWARE\\**Wow6432Node**\Microsoft\Windows\CurrentVersion\Explorer\FolderDescriptions

#### 删除百度云盘和迅雷影片库

```
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace\
```

这个位置下面删除相应的文件夹