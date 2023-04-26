---
layout: post
title:  "Linux中安装多个python环境"
date:   2023-04-25 
description: "Linux中安装多个python环境"
tag: Linux
---

# 查看现在的python安装位置

```shell
hugo@hugo-virtual-machine:~/Python-3.8.0a4$ which python
/home/hugo/anaconda3/bin/python
hugo@hugo-virtual-machine:~/Python-3.8.0a4$ which python3.7
/usr/bin/python3.7
hugo@hugo-virtual-machine:~/Python-3.8.0a4$ which python3.10
/usr/bin/python3.10
```

## 安装Python3.8

```shell
sudo apt-get install python3.8
```

安装失败，应该是ppa源的问题，需要更换源

```shell
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python3.8
```

还是失败，没办法更换源。

从源头开始安装，先下载源码包

```shell
wget https://www.python.org/ftp/python/3.8.0/Python-3.8.0a4.tgz
```

解压

```shell
tar -zxvf Python-3.8.0a4.tgz
```

进入解压后的目录

```shell
cd Python-3.8.0a4
```

配置

```shell
./configure --prefix=/usr/local/python3.8
```
./configure这段代码是用来检查系统环境是否满足编译安装Python的要求，如果不满足，会提示你缺少哪些依赖包，需要你自己去安装，安装完成后再次执行./configure，直到没有提示信息，这时候就可以执行make命令了。

--prefix=/usr/local/python3.8这段代码是用来指定Python的安装路径，如果不指定，默认安装在/usr/local/bin目录下。

编译

```shell
make -j 8
```

-j 8表示使用8个线程进行编译，这样可以加快编译速度，如果你的电脑是双核的，可以使用-j 2，如果是四核的，可以使用-j 4，以此类推。

查看核心数

```shell
hugo@hugo-virtual-machine:~/Python-3.8.0a4$ cat /proc/cpuinfo | grep "processor" | wc -l
4
```

安装

```shell
sudo make install
```

编译安装同时进行，可以使用

```shell
make -j 4 && sudo make install
```

安装完成后，查看安装位置

```shell
hugo@hugo-virtual-machine:~/Python-3.8.0a4$ which python3.8
/usr/local/python3.8/bin/python3.8
```
