---
title: 'Linux常用命令'
tags: 
-
categories: 
- Linux
date: "2020-03-19T16:18:09+08:00"
comment:  true    
---

### 查看文件大小

- stat命令

  - ```bash
    (base) root@debian:~/tf_down# stat download
      File: download
      Size: 9087      	Blocks: 24         IO Block: 4096   regular file
    Device: fe01h/65025d	Inode: 410261      Links: 1
    Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
    Access: 2020-03-19 16:14:06.998596373 +0800
    Modify: 2020-03-19 16:14:07.102598211 +0800
    Change: 2020-03-19 16:14:07.102598211 +0800
     Birth: -
    
    ```

- wc命令

  - ```bash
    (base) root@debian:~/tf_down# wc -c download
    9087 download
    
    ```

- du - b 命令

  - ```bash
    (base) root@debian:~/tf_down# du -b download
    9087	download
    ```

- ls 命令

  - ```bash
    (base) root@debian:~/tf_down# ls -l download
    -rw-r--r-- 1 root root 9087 Mar 19 16:14 download
    
    ```

  - ```bash
    (base) root@debian:~/tf_down# ls -lh download
    -rw-r--r-- 1 root root 8.9K Mar 19 16:14 download
    
    ```