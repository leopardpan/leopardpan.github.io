---
layout: post
title: Ubuntu创建pycharm的快捷方式两种方法
date: 2023-08-09
description: "Ubuntu创建pycharm的快捷方式两种方法"
tag: Linux
---

## 方法一

1. 打开终端，输入命令：`sudo gedit /usr/share/applications/pycharm.desktop`，创建pycharm.desktop文件
2. 在pycharm.desktop文件中输入以下内容：

```txt
[Desktop Entry]
Type=Application
Name=Pycharm
GenericName=Pycharm3
Comment=Pycharm3:The Python IDE
Exec=sh /home/hugo/Downloads/pycharm-community-2023.1.3/bin/pycharm.sh 
Icon=/home/hugo/Downloads/pycharm-community-2023.1.3/bin/pycharm.png
Terminal=pycharm
Categories=Pycharm;
```

## 方法二

1. 打开pycharm所在目录，运行命令：`./bin/pycharm.sh`，打开pycharm;
2. 点击tools;
3. 点击Create Desktop Entry;
4. add to favorites;

