---
title: "Debian安装python38"
date: 2019-12-07T22:19:52+08:00
categories:
- Python
- Debian
tags:
- python安装
- debian
keywords:
- python
#thumbnailImage: //example.com/image.jpg
---



#### <font color=#CC3300>源码安装</font>

##### 1、安装依赖包

```shell
sudo apt-get install build-essential python-dev python-setuptools python-pip python-smbus build-essential libncursesw5-dev libgdbm-dev libc6-dev zlib1g-dev libsqlite3-dev tk-dev  libssl-dev openssl  libffi-dev
```



##### 2、下载python3.8源码

```shell
##更新
sudo apt-get update
##下载
wget https://www.python.org/downloads/release/python-380/
##解压
tar -xvf Python-3.8.0.tar.xz
cd  Python-3.8.0/
```

##### 3、配置，编译，安装

> 源码安装的3个步骤，配置（configure），编译(make)，安装(make install)
>
> Configure是一个可执行脚本，它有很多选项，在待安装的源码路径下使用命令./configure –help输出详细的选项列表。其中**--prefix选项是配置安装的路径。**
>
> 如果不配置 --prefix 选项，安装后：
>
> 可执行文件默认放在/usr /local/bin，
>
> 库文件默认放在/usr/local/lib，
>
> 配置文件默认放在/usr/local/etc，
>
> 其它的资源文件放在/usr /local/share，
>
> `--enable-optimizations` 是优化选项（LTO，PGO 等）加上这个 flag 编译后，性能有 10% 左右的优化（如果没记错的话），但是这会明显的增加编译时间



```shell
./configure --prefix=/usr/local/python38 --enable-optimizations
make && make install
```

##### 4、创建软链

```python
ln -s /usr/local/python3.8/bin/python3.8 /usr/bin/python38

ln -s /usr/local/python3.8/bin/pip3.8 /usr/bin/pip38
```

