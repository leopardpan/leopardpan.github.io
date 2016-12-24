---
layout: post
title: Mac知识整合
date: 2016-11-16 
tags: iOS    
---

### .DS_Store 文件是什么？

.DS_Store 是 Mac OS 保存文件夹的自定义属性的隐藏文件，如文件的图标位置或背景色，相当于 Windows 的 desktop.ini。

1，禁止.DS_store 生成：             
打开 “终端” ，复制黏贴下面的命令，回车执行，重启Mac即可生效。

```
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool TRUE
```

2，恢复.DS_store生成：

```
defaults delete com.apple.desktopservices DSDontWriteNetworkStores
```

### 显示隐藏文件

在终端执行命令，显示隐藏文件

```
defaults write com.apple.finder AppleShowAllFiles -bool true
```

恢复隐藏

```
defaults write com.apple.finder AppleShowAllFiles -bool false
```

执行命令后需要重新打开能看到效果。


### 切换 Pyhton 环境

我本地之前 Python 环境是 2.7.10 ，然后学习 Tensorflow 的时候，安装了 Python 3.5.2 ，把系统默认 Pyton 环境也设置成了 3.5.2 版本，今天运行以前写的 python 脚本发现运行不了了，因为python 2.7 和 3.5 的 语法有挺多改动，现在我需要把系统的 python 环境回退到 2.7。


可以直接修改 `~/.bash_profile` 文件。

* 1、修改 `vim ~/.bash_profile`

```
修改方式有很多种，使用 vim  ，或者 cd ~/ 然后 open . 打开文件夹，找到 .bash_profile 文件，双击打开。
```

* 2、在`.bash_profile` 文件里添加下面参数 

```
alias python="/System/Library/Frameworks/Python.framework/Versions/2.7/bin/python2.7"
```

* 3、使用命令 `source ~/.bash_profile` 或者重启 终端 就 OK 了 。


现在你再在终端输入 `python` 就会发现，显示的信息为 2.7 了

```bash

Python 2.7.10 (default, Oct 23 2015, 19:19:21) 
[GCC 4.2.1 Compatible Apple LLVM 7.0.0 (clang-700.0.59.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.

```


### 生成SSHKey过程

```

1.查看是否已经有了ssh密钥：`cd ~/.ssh` ，如果没有密钥则不会有此文件夹，有则备份删除。    
2.生存密钥：ssh-keygen -t rsa -C “test@gmail.com”。   按3个回车，密码为空。       

Your identification has been saved in /home/tekkub/.ssh/id_rsa.
Your public key has been saved in /home/tekkub/.ssh/id_rsa.pub.
The key fingerprint is:
………………    
最后得到了两个文件：id_rsa和id_rsa.pub  

```

### 使用版本控制器 SVN (versions) 添加.a库 

Xcode 自带的 svn 和 Versions 以及一些其它工具都不能上传".a"文件

下面是在 Mac 上如何把 .a 添加到 SVN 里面的

1、打开终端，输入cd，空格，然后将需要上传的 .a 文件所在的文件夹（不是.a文件） 拖拽到终端（此办法无需输入繁琐的路径，快捷方便） 回车

2、之后再输入如下命令：`svn add libGoogleAnalytics.a` ，回车

之后会出现：A (bin) libGoogleAnalytics.a

表示添加成功，打开 Versions 就可以看到，刚才添加的 .a 文件，此时就可以手动上传了。 

另外，请注意路径的正确性。



转载请注明：[潘柏信的博客](http://baixin) » [点击阅读原文](http://baixin.io/2016/11/macTips/)     









