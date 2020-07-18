---
title: 'Git不能锁定ref'
tags: 
-
categories: 
- Git
date: "2020-04-06T16:16:08+08:00"
comment:  true    
---

前进！前进！！不择手段的前进！

<!--more-->

### `error: update_ref failed for ref 'refs/remotes/origin/master': cannot lock ref 'refs/remotes/origin/master': unable to resolve reference 'refs/remotes/origin/master': reference broken`

今天莫名奇妙，就报了这个错误。不能锁定ref...

查了一圈攻略，反正就是`./.git`文件夹中的文件出了错误，

### 解决方法：

重新clone下来，复制`./.git`文件夹下的全部文件，然后覆盖本地的文件。然后在`pull`下来,最后就好了。