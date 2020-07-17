---
title: 'Sudo切换用户'
tags: 
-
categories: 
- Linux
date: "2020-07-17T23:34:58+08:00"
comment:  true    
---

与你常在

<!--more-->

sudo 命令可以让普通用户执行root级别的命令

配置过程：

如果没有安装sudo，则需要先切换到root用户 apt-get install sudo

安装完成之后，通过修改/etc/sudoers 文件，在之中添加普通用户即可。但是/etc/sudoers文件，不能随便修改

要通过visudo工具修改，visudo可能不在PATH中，所以还要将visudo添加到PATH中[参考](https://blog.csdn.net/Zsigner/article/details/88863745)

```shell
$ whereis visudo
visudo: /usr/sbin/visudo /usr/share/man/man8/visudo.8.gz
$ echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
将visudo添加进PATH中
$ ln -s /usr/sbin/visudo /usr/bin/visudo
```

最后通过修改sudoers文件，在最好添加上用户

```shell
root    ALL=(ALL:ALL) ALL
haillk ALL=(ALL:ALL) ALL
%代表后面接的是用户组
%haillk ALL=(ALL) ALL
NOPASSWD指的是不需要密码
%haillk ALL=(ALL) NOPASSWD
```

