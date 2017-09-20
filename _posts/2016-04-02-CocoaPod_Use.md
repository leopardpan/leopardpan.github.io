---
layout: post
title: CocoaPods使用心得
date: 2016-04-02
tag: iOS 
--- 

### 简介：   
　本章介绍什么是 `CocoaPods` ,如何使用 `CocoaPods` , 以及 `CocoaPods` 的原理,和使用 `CocoaPods` 时经常出现的一些问题。

　Cocoapods 是 OS X 和 iOS 下的一个第三方库管理工具。我们能使用CocoaPods添加被称作 “Pods”的依赖库,并轻松管理它们的版本,CocoaPods会帮我们配置好这些三方库的路径及开发环境,极大的提升了开发者的工作效率。


### 安装CocoaPods　    

　Mac下自带ruby,使用ruby的gem命令安装,ruby的软件源被墙了,把官方的ruby源替换成国内的淘宝源。

### 更换Gem源   

```bash
$ gem sources --remove https://rubygems.org/
$ gem sources -a https://ruby.taobao.org/
$ gem sources -l
```
* 1.移除掉原有的源（服务器在国外，速度较慢）。
* 2.等1有反应之后再敲2命令（替换成淘宝上的ruby镜像https）。
* 3.验证是否成功。成功如下：

```bash

*** CURRENT SOURCES ***

http://ruby.taobao.org/

```

### 更新Gem源

```bash

sudo gem update --system

```

### 安装cocoapods        

```bash

$ sudo gem install cocoapods
$ pod setup

```

pod setup 在执行时会比较慢，因为Cocoapods 要将它的信息下载到 ~/.cocoapods目录下, 耐心等待…


#### 提升cocoapods的安装速度

所有的项目的 Podspec 文件都托管在https://github.com/CocoaPods/Specs。第一次执行 pod setup 时，CocoaPods 会将这些podspec索引文件更新到本地的 ~/.cocoapods/目录下，这个索引文件比较大，有 80M 左右。
作者akinliu 在 gitcafe 和 oschina 上建立了 CocoaPods 索引库的镜像(在国内),我们可以使用CocoaPods国内的镜像索引，操作时会快多了,如gitcafe：

```bash

pod repo remove master
pod repo add master https://gitcafe.com/akuandev/Specs.git
pod repo update

```


### 使用cocoapods

cocoapods安装完成后，使用 pod search 命令来验证一下

```bash

pod search AFNetworking


```

终端将会有如下结果：

```bash

-> AFNetworking (3.0.4)
A delightful iOS and OS X networking framework.
pod 'AFNetworking', '~> 3.0.4'
- Homepage: https://github.com/AFNetworking/AFNetworking
- Source:   https://github.com/AFNetworking/AFNetworking.git
- Versions: 3.0.4, 3.0.3, 3.0.2, 3.0.1, 3.0.0, 3.0.0-beta.3, 3.0.0-beta.2,
3.0.0-beta.1, 2.6.3, 2.6.2, 2.6.1, 2.6.0, 2.5.4, 2.5.3, 2.5.2, 2.5.1, 2.5.0,
2.4.1, 2.4.0, 2.3.1, 2.3.0, 2.2.4, 2.2.3, 2.2.2, 2.2.1, 2.2.0, 2.1.0, 2.0.3,
2.0.2, 2.0.1, 2.0.0, 2.0.0-RC3, 2.0.0-RC2, 2.0.0-RC1, 1.3.4, 1.3.3, 1.3.2,
1.3.1, 1.3.0, 1.2.1, 1.2.0, 1.1.0, 1.0.1, 1.0, 1.0RC3, 1.0RC2, 1.0RC1,
0.10.1, 0.10.0, 0.9.2, 0.9.1, 0.9.0, 0.7.0, 0.5.1 [master repo]
- Subspecs:
- AFNetworking/Serialization (3.0.4)
- AFNetworking/Security (3.0.4)
- AFNetworking/Reachability (3.0.4)
- AFNetworking/NSURLSession (3.0.4)
- AFNetworking/UIKit (3.0.4)


-> AFNetworking+AutoRetry (0.0.5)
Auto Retries for AFNetworking requests
pod 'AFNetworking+AutoRetry', '~> 0.0.5'
- Homepage: https://github.com/shaioz/AFNetworking-AutoRetry
- Source:   https://github.com/shaioz/AFNetworking-AutoRetry.git
- Versions: 0.0.5, 0.0.4, 0.0.3, 0.0.2, 0.0.1 [master repo]

.........太多了，省略

```

pod search 是CocoaPods的一个搜索命令,我们可以用来搜索任何托管在CocoaPods上的三方库。    

使用CocoaPods时需要新建一个 Podfile 的文件,cd 到 我的Demo项目里，Demo目录下有三个文件

```bash

Demo 、  Demo.xcodeproj  、 DemoTests

```

新建 Podfile

```bash

touch Podfile

```

vim 编辑 Podfile

```bash
vim Podfile
```
由于是新建的 Podfile 里面应该是空白的。然后我们在里面添加依赖库，格式如下：

```bash

platform :ios
pod 'Reachability',  '~> 3.0.0'
pod 'ASIHTTPRequest'

```

‘~> 3.0.0’ 是 Reachability 的版本号, 设定了版本号CocoaPods就会下载对应的版本,ASIHTTPRequest没指定版本号,CocoaPods就会下载最新版本的ASIHTTPRequest。
退出编辑，执行 pod install 下载三方库。

```bash

pod install

```
完成后我Demo项目下的文件多了几个:

```bash
Demo 、  Demo.xcodeproj  、 DemoTests （之前的三个）

Demo.xcworkspace 、Podfile 、Podfile.lock 、Pods
```

这个时候我们打开Demo项目是点击 Demo.xcworkspace 文件了，到此CocoaPods的基本使用已经讲完了，接下来的CocoaPods的原理，和让我们自己的三方库也支持CocoaPods。

待续…

[深入理解 CocoaPods](http://blog.jobbole.com/53365/)    

<br>

转载请注明：[潘柏信的博客](http://baixin) » [点击阅读原文](http://baixin.io/2016/04/CocoaPod_Use/)     







