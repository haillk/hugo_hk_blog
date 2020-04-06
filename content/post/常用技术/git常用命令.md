---
title: "Git常用命令"
date: 2019-12-21T19:17:53+08:00
lastmod: 2019-12-21T19:17:53+08:00
show_in_homepage: true

tags: ['git常用命令']
categories: ['Git']

featured_image: ''
featured_image_preview: ''

comment: true
toc: true
autoCollapseToc: true
---

<!--more-->

## 基础命令：
branch相关

```powershell
##创建分支
$git branch bran-1
##切换分支
$git checkout bran-1
##创建并切换，是前两条命令的合并
$git checkout -b bran-1
##删除分支
$git branch -d bran-1
```

链接远程库

```powershell
git remote add origin github.com///
```



### 问题1：发现提交错了，要回退版本

```powershell
##这条命令能列出所有的操作记录（commit ，clone）
$git reflog
010df8d HEAD@{20}: commit: love3
5488a92 HEAD@{21}: commit: love2
753ec32 HEAD@{22}: commit: love it
bc372b2 HEAD@{23}: commit: e
8fa2f36 HEAD@{24}: commit: rm modules
96b103b HEAD@{25}: commit: theme3
##然后选择想会退的版本，回退。
$git reset HEAD@{index}
```

### 问题2：想改个小东西，但是代码已经提交了

```powershell
##首先，添加当前已改动的代码
$git add .
##这条命令会把代码合并到上一次提交
$git commit --amend
```

### 问题3：上次commit的记录写得不够好，要重写一下。

```powershell
$git commit --amend
```

### 问题4：代码提交到错误的分支上了

```powershell
##先撤销错误分支的最后一次提交
$git reset HEAD~ --soft
##再保存本地修改
$git stash
##切换到正确分支
$git check name-of-corrent
##释放本地修改
$git pop stash pop
$git add .
$git commit -m "message"
```

当然也有逼格高一点的方式。

```powershell
##先切换到正确的分支
$git checkout name-of-corrent
##然后使用cherry-pick 来获取最新的一条提交记录
$git cherry-pick name-of-error
##最后把错误分支上的错误记录删除
$git checkout name-of-error
$git reset HEAD~ --hard
```

