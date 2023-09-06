---
layout: post
title:  "修改Windows中 pip 的缓存位置与删除 pip 缓存"
date:   2023-04-19
desc: "修改Windows中 pip 的缓存位置与删除 pip 缓存"
tags: Python
---
# 修改Windows中 pip 的缓存位置与删除 pip 缓存

下载安装Python 库时，是安装版的话一般都通过 pip install xxx 来安装包。但安装下载的文件都会缓存下来，而且默认都在C盘。最近C盘要G了，导致好多包安装失败。

哪怕是下载失败，缓存也会默认放在C盘中

### 所以改变pip缓存位置

首先先建立一个文件夹来充当缓存目录,比如说就在D盘下面的pipCache目录。

WIN+R命令行窗口

```shell
pip config set global.cache-dir "D:\pipCache"
1
```

> 注：文件夹路径地址要如我这样的绝对地址。

![20230419095403](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230419095403.png)

如果出现第二行的语句，说明更改成功。

然后就可以删除之前的pip缓存了，也可以把其复制来这个新地址。



### 删除 pip 缓存

在文件资源管理器（这个指的是最上面文件路径）输入下面语句回车。

```shell
%LocalAppData%
```


找到pip文件夹删除即可

### 配置pip.ini文件

在文件资源器软件的路径框输入 `%APPDATA%` 回车

在 `Roaming`文件夹下找到 `pip`文件夹，**如果没有就新建一个** `pip.ini`文件

> 如果后面测试有问题，可能是系统自带笔记本编码问题。就改用你写代码的编辑器新建 `pip.ini`文件，要utf-8的编码。
>
