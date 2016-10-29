---
layout: post
title: Xcode 8 使用笔记
date: 2016-10-25 
tags: iOS    
---

最近使用 Xcode8遇到了一些问题，想记下来，发现简书上有位同学写了一篇很详细的教程 [原文链接](http://www.jianshu.com/p/c1904fd8db06)，比较懒惰的我就在他的基础上加了些我自己的一些笔记。


### Interface Builder

随着 14 年的 iPhone6 和 6P 出来之后，iPhone 的屏幕尺寸也越来越多，屏幕适配是一个需要解决的问题，以后不一定苹果又出什么尺寸的 iPhone 呢。

在 iPhone6 和 6P 发布的同一年，苹果推出的 Xcode6 中在原有的 Auto layout的基础上，添加了Size Classes新特性，通过这个新特性可以使用一个XIB或者SB文件，适配不同的屏幕以及iPhone和iPad两种设备。

在 Xcode8 中，苹果推出了更加强大的可视化编辑工具预览功能，可以在不运行App的情况下，预览当前XIB或SB在不同屏幕尺寸下的显示。(这个功能我记得之前Xcode就有，只是隐藏的比较深，苹果现在给拿到外面了)

选择一个XIB文件进去，点击下面红框的位置，会出现从3.5寸-5.5寸一系列屏幕尺寸的选项。直接点击不同屏幕尺寸，以及横竖屏选项，切换不同的屏幕显示。在iPad上还可以选择是否分屏，功能非常强大。


<img src="/images/posts/Xcode8/image1.png" height="200" width="600"> 

在右边有一个 Vary for Traits 选项，点击这个选项就可以同时显示所有可选的屏幕样式，功能和上面图片都一样，只是显示上看起来比较多。


<img src="/images/posts/Xcode8/image2.png" height="160" width="600">  

还有一点，新创建的 XIB 控件尺寸，不再是之前 600*600 的方块了，而是默认是6s的长方形 XIB 文件，看起来舒服多了。

### Target中General 的变化

在 Xcode8 之前，都需要自己设置证书和描述文件。如果设置出现错误的情况下，还可以通过点击 Fix issue 来修复这个错误。但这有个问题就在于，Fix issue 选项并不是那么好用，有的时候设置是正确的这里也提示需要 Fix issue。

可能苹果也意识到这个问题的存在，在Xcode8中可以通过Automatically manage signing选项，让苹果为我们管理证书和配置文件，设置也都是由苹果来完成的。在Xcode8中新建项目，这个选项默认是被勾选的。


<img src="/images/posts/Xcode8/image3.png" height="350" width="500">  


从上面图中可以看到，苹果帮我们自动管理了证书和配置文件。而且在之前的项目中，如果想要设置安装后显示在手机上的App名字，还需要自己到Info.plist文件中，修改Display Name字段，而现在直接在General中就可以做修改，这个修改和Info.plist是同步的。

但是，如果我想自己管理证书和描述文件呢？只需要去掉Automatically manage signing选项。


<img src="/images/posts/Xcode8/image4.png" height="350" width="500"> 

如果自己到Build Settings中手动设置证书和描述文件，可以发现Provisioning Profile选项已经被标明为Deprecated，也就是苹果并不推荐手动设置。

### Xcode 插件

升级 Xcode8 之后会发现，在 Xcode8 中所有第三方插件都失效了，并且连之前菜单栏的插件选项也不存在了。在之前很多 iOS 开发者，都是通过 [Alcatraz](http://alcatraz.io/) 来管理插件的，现在 Alcatraz 也是不可用的。但是X code8 自身也对编译器进行了升级，将一些比较好的插件功能加入到 Xcode 中，例如单行高亮显示等。

在 Xcode8 中支持了开发插件工程，并且为我们提供了一个插件模板，开发的插件可以上传到App Store 下载。苹果这么做有一个原因在于，之前 Xcode和插件是运行在同一个进程的，所以插件的崩溃也会导致Xcode崩溃。苹果现在将插件作为一个单独的应用程序，分开进程运行，不会对Xcode带来其他影响。

<img src="/images/posts/Xcode8/image5.png" height="350" width="500">  

### Runtime Issues

在开发过程中，因为语法或明显的代码错误(例如Retain Cycle)，编译器可以发现并报黄色或红色警告。但是一些因为代码逻辑导致的错误，编译器并没有办法找到。例如下面的这句代码，因为代码逻辑的问题导致两个数组相互引用，都不能释放。

<img src="/images/posts/Xcode8/image6.png" height="180" width="600">  

这时候可以通过 Xcode8 提供的 Runtime Issues 新特性，查找到运行过程中出现的问题，并通过 Graph 的方式将问题可视化的展现给开发者。

<img src="/images/posts/Xcode8/image7.png" height="300" width="600"> 

### Debug Memory Graph

在Xcode6中出现了Debug View Hierarchy新特性，可以通过其调试当前App的视图层级，查找UI相关的bug非常方便。在Xcode8中苹果为开发者提供了Debug Memory Graph特性，通过这个新特性，可以直接选择一个对象，查看与其相关的内存关系。


<img src="/images/posts/Xcode8/image8.png" height="300" width="600"> 

Debug Memory Graph 和 Runtime Issues 可以配合使用，通过 Debug Memory Graph 分析内存关系完成后，点击 Runtime Issues 可以看到已经发现的内存问题。

### Swift 3

Xcode8 带来了新版本的 Swift3，新版本的Swift变化较大，如果旧版的Swift项目在Xcode8上编译可能会失败。对此，苹果为开发者提供了Swift迁移工具，听说不太好用(我没用过这个工具)。

如果不想立刻就迁移到Swift3，可以在Builder Settings中进行设置，选择Use Legacy Swift Language Version设置为YES，就可以继续使用旧版本的Swift2.3。

<img src="/images/posts/Xcode8/image9.png" height="300" width="600">   

### 其他更新

Xcode 新版字体，SF Mono Regular 字体。更新 Xcode 之后我比较喜欢这种字体，看起来代码非常工整。
被编辑的行高亮显示。之前Xcode有个插件就是这个功能，Xcode8把高亮功能集成进来了，使用起来很方便。
最新版的API文档，展示样式发生了很大的改变。
更方便的生成文档(就是喵神写的VVDocumenter)，在Xcode8中可以将光标放在方法上面，通过option + command + /快捷键生成文档注释。

### Xcode8适配,XIB和Storeboard适配

在 Xcode8 之前，创建一个 XIB 或 SB 文件，都是一个 600*600 的方块 XIB 文件。在 Xcode8 之后，创建的 XIB 文件默认是6s尺寸的大小。

但是 Xcode8 打开之前旧项目的 XIB或SB 文件时，会弹出下面的弹框， 这时候一般直接选择Choose Device即可。


<img src="/images/posts/Xcode8/image10.png" height="300" width="500">  

但是这样有个问题，如果Xcode8打开过这个XIB文件，并选择Choose Device之后。其他的Xcode8以下版本的编译器，将无法再打开这个文件，会报以下错误：

The document “ViewController.xib” requires Xcode 8.0 or later. This version does not support documents saved in the Xcode 8 format. Open this document with Xcode 8.0 or later.
有两种方法解决这个问题：

你同事也升级Xcode8，比较推荐这种方式，应该迎接改变。
右击XIB或SB文件 -> Open as -> Source Code，删除xml文件中下面一行字段。
<capability name="documents saved in the Xcode 8 format" minToolsVersion="8.0"/>

### 编译错误

升级Xcode之后，Xcode8对之前的一些修饰符和语句不兼容，会导致一些编译错误。这种错误导致的原因很多，这里大致列几条，各位还是根据自身遇到的情况做修改吧。

之前一些泛型相关的修饰符，nullable之类的有的会报错。
CAAnimation及其子类，设置代理属性后，必须在@interface()遵守代理，否则报错，等等。

### 权限适配

这应该算iOS10系统适配的范畴，最近这两个都在弄，所以就直接和Xcode8适配一起写出来了。

在iOS10之后需要在Info.plist中，添加新的字段获取权限，否则在iOS10上运行会导致崩溃。下面是一些常用的字段，如果有缺少的麻烦各位评论区补充一下。

Key	权限
Privacy - Camera Usage Description	相机
Privacy - Microphone Usage Description	麦克风
Privacy - Photo Library Usage Description	相册
Privacy - Contacts Usage Description	通讯录
Privacy - Bluetooth Peripheral Usage Description	蓝牙
Privacy - Location When In Use Usage Description	定位
Privacy - Location Always Usage Description	后台定位
Privacy - Calendars Usage Description	日历

参考资料：[developer.apple](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html)

### 推送通知

苹果的推送在之前iOS8和iOS9的时候就发生过大的更新，推送功能越来越强大。在iOS10之后苹果推出了UserNotifications框架，可以通过这个框架更好的控制推送通知，可以更新、修改锁屏页面的推送消息，可以添加图片等功能。

但是在用Xcode8打包后，并且不对代码进行修改的情况下，会发现打包后苹果发来了一封邮件。这封邮件大概意思是如果需要使用推送通知，需要对代码做修改，否则将不能使用推送通知。


<img src="/images/posts/Xcode8/image11.png" height="300" width="600">  

这是因为在Xcode8之后，如果需要使用Push Notifications的功能，需要勾选Capabilities -> Push Notifications为YES，否则进行远程推送就会有问题，并且会收到苹果发来的这封邮件。

### 删除系统log

升级Xcode8之后，在调试和运行过程中，发现控制台打印了很多不认识的log，这些log是系统打印的，和开发者没关系。但是这么多log看着比较乱，怎么屏蔽掉呢？

subsystem: com.apple.UIKit, category: HIDEventFiltered, enable_level: 0, persist_level: 0, default_ttl: 0, info_ttl: 0, debug_ttl: 0, generate_symptoms: 0, enable_oversize: 1, privacy_setting: 2, enable_private_data: 0
在Target -> Edit Scheme -> Run -> Arguments中，添加OS_ACTIVITY_MODE字段，并设置为Disable即可。

<img src="/images/posts/Xcode8/image12.png" height="300" width="500">   

顺便提一下，这两天在设置log选项的时候，发现可以通过在Arguments中设置参数，打印出App加载的时长，包括整体加载时长，动态库加载时长等。

在Environment Variables中添加DYLD_PRINT_STATISTICS字段，并设置为YES，在控制台就会打印加载时长。

<img src="/images/posts/Xcode8/image13.png" height="300" width="600">  

### awakeFromNib报警告

老项目在Xcode8中，有些重写awakeFromNib方法的地方，会报下面的错误。这是因为没有调用super的方法导致的，还好我平时都是调用super的，我代码目前还没出问题。

```
Method possibly missing a [super awakeFromNib] call
```





