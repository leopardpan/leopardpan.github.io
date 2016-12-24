---
layout: post
title: "iOS 9 变化笔记"
date: 2015-09-26 18:15:06 
description: "iOS9 变化笔记, 以及工作中常遇到的问题"
tag: iOS
---


这里将介绍下我们日常开发一些从iOS8过度到iOS9给我们带来的一些变化，及解决方法。
     

### App Transport Security

iOS9和OS X El Capitan的一个新特性，App Transport Security 的目地是提高Apple 操作系统的安全性以及在此操作系统上运行的任何应用的安全性。ATS是苹果针对与 NSURL这一层做的封装，iOS9后ATS默认是开启的，即网络传输需要使用HTTPS。如果想在iOS9后继续使用HTTP的话，有两条路可以走：

> 1. 在Info.plist中添加 `NSAppTransportSecurity`类型Dictionary，在`NSAppTransportSecurity`下添加`NSAllowsArbitraryLoads`，Boolean 为 YES。
> 2. 直接使用CFNetwork做网络请求，ASIHTTPRequest就是基于CFNetwotk做的封装，如果有需求的同学可以看看ASI里面的源码，如果某个时间段你又想要使用HTTPS的话，ASI对SSL/TSL的证书验证有点问题，证书验证还得自己封装一下才行。刚才我说道，ATS是苹果针对与NSURL这一层做的封装，所以我们使用CFNetwork或者更底层做网络请求的话是不受ATS限制的。


### 移除了discoveryd DNS解析服务

iPhone升级到iOS8后WiFi有时候会有问题，特别是Mac升级到OS X Yosemite后，时而电脑休眠唤醒唬就连不上WiFi，有时候还突然掉线，经常要手动去关闭WiFi在重新连接，这是因为苹果到了OS X Yosemite系统后，把之前的mDNSResponder换成了discoveryd DNS。iOS9和OS X Yosemite10.4后mDNSResponder又回来了。

mDNSResponder： 苹果以前一直使用控制DNS和Bonjour服务的一种进程。
discoveryd：OS X Yosemite后苹果新出的一种进程。

### App Thinning
App Thinning是一个关于节省iOS设备存储空间的功能，它可以让iOS设备在安装、更新及运行App等场景中仅下载所需的资源，减少App的占用空间，从而节省设备的存储空间。

#### App Thinning主要有三个机制：

> 1. Slicing： 开发者把App安装包上传到AppStore后，Apple服务会自动对安装包切割为不同的应用变体(App variant)， 当用户下载安装包时，系统会根据设备型号下载安装对应的单个应用变体。
> 2. On-Demand Resources： ORD(随需资源)是指开发者对资源添加标签上传后，系统会根据App运行的情况，动态下载并加载所需资源，而在存储空间不足时，自动删除这类资源。
> 3. Bitcode：开启Bitcode编译后，可以使得开发者上传App时只需上传Intermediate Representation(中间件)，而非最终的可执行二进制文件。 在用户下载App之前，AppStore会自动编译中间件，产生设备所需的执行文件供用户下载安装。

其中，Bitcode的机制可以支持动态的进行App Slicing，而对于Apple未来进行硬件升级的措施，此机制可以保证在开发者不重新发布版本的情况下而兼容新的设备。Xcode7默认是开始了Bitcode，如果不想使用可以手动关闭Bitcode：



* 选择项目——>点击Target——>点击Build Setttings——>搜索栏里搜bitcode——>把Enable Bitcode对应的Yes改成No。


启用Bitcode编译机制，需要注意以下几点：

1. 如果应用开启Bitcode，那么其集成的其他第三方库也需要是Bitcode编译的包才能真正进行Bitcode编译
2. 开启Bitcode编译后，编译产生的.app体积会变大(中间代码，不是用户下载的包)，且.dSYM文件不能用来崩溃日志的符号化（用户下载的包是Apple服务重新编译产生的，有产生新的符号文件），使用dSYM来收集Crash日志的同学得注意了。
3. 通过Archive方式上传AppStore的包，可以在Xcode的Organizer工具中下载对应安装包的新的符号文件
 

### 后台定位

iOS9后苹果为了对保障用户的地理位置的隐私对App请求后台定位有了权限设置，则需要多加一些代码。如果不适配iOS9，就不能偷偷在后台定位，如果没有后台定位的权限也是可以在后台定位的，只是会出现蓝条。

开启后台定位功能：`locationManager.allowsBackgroundLocationUpdates = YES;`
locationManager是CLLocationManager的对象，用来管理整个定位的。

**重点：**

> 配置info.plist，添加一个Required background modes，Array类型的，然后在Required background modes里面Item 0对应的Value设置为App registers for location updates，这样就解决了iOS9后台定位出现蓝条的问题了。


### UI Testing
Xcode7中苹果引入了一种新的方式在应用中进行测试——UI Testting，UI Testting允许我们找到UI元素与之交互，还能检查属性和状态。UI Testting已经完全集成进了Xcode7的测试报告，可以和单元测试一起执行。使用起来跟之前Xcode5出来的XCTest差不多，Xcode bots提供对此的支持，而且command line支持当UI测试失败时会立即发出通知。

可以参考Github上的Demo，步骤：

1. 在DemoTests.m里创建一个test开头的方法
2. 在setUp()里启动应用 `XCUIApplication().launch()`
3. 新建一个方法test开头的，在里面获取应用`let app = XCUIApplication()`
4. 的到`let app = XCUIApplication()`，a`pp.buttons[“View Detail”].tap()?`。buttons是当前这个界面的所有按钮的集合，[]里面写按钮的名字，tap()就是执行这个按钮所对应的方法，可以是网络请求、界面跳转等等。


### URL scheme

在iOS9中，如果使用URL scheme必须在"Info.plist"中将你要在外部调用的URL scheme列为白名单，否则不能使用。

配置info.plist，添加一个`LSApplicationQueriesSchemes`，Array类型的，然后在`LSApplicationQueriesSchemes`的Item里面添加urlscheme就行了，urlscheme是任意一个字符串，就是你自己需要使用的urlscheme，iOS9 URL scheme白名单适配就完成了。

### 出现大量的警告

Xcode7后运行以前的项目后出现大量的警告如：

```bash
(null): warning: /var/folders/p4/z7zy68r92hd3p5ry5g2v3k_8rlwzzr/C/org.llvm.clang.dalmo/ModuleCache/1TXZDLI9N2EMV/Foundation-3DFYNEBRQSXST.pcm: No such file or directory。
```

作为一个有洁癖的我反正是不能忍，出现警告的大致原因跟我上面提到的开启Bitcode，.dSYM文件不能用来符号化有关，Xcode试图去创建dSYM文件，但是你又不需要。

### 解决方法

1. Build Settings ——>Build Options——>Debug Information Format
2. Debug下的DWARF with dsYM File改成DWARF
3. Release下的还是之前默认的DWARF with dsYM File不变
 

参考资料：

- [iOS9AdaptationTips](https://github.com/ChenYilong/iOS9AdaptationTips) 
- [iOS9学习系列](http://www.cocoachina.com/ios/20150821/13140.html) 
- [iOS9-day-by-day](https://github.com/shinobicontrols/iOS9-day-by-day)



<br>

转载请注明：[潘柏信的博客](http://baixin) » [点击阅读原文](http://baixin.io/2015/09/iOS9_Note/)