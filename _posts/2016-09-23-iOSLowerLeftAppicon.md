---
layout: post
title: "iOS设备左下角出现Appicon"
date: 2016-09-23 
tags: iOS   
---

最近发现我设备锁屏后，按Home屏幕变亮的时候，左下角出现一个灰色的Appicon （应用图标），关于这个应用图标的出现做了一些调研，下面是应用图标出现的几种情况。

图一 iOS 系统自带的 App icon , 图二 第三方 App icon , 图三 通过 iBeacon 信号激活的 demo icon

![](/images/posts/icon/image01.png)

## 结论：有三种情况导致设备的左下角出现灰色的 App icon

### 1、AppStore根据地点对App 推荐
* **简介** 
	* iOS 8会基于你的位置在锁屏界面上展示一个app快捷打开方式。比如你正在星巴克附近，那iOS 8会在锁屏界面上展示星巴克应用的icon，方便你快速打开。一些用户也表示会在锁屏界面收到app推荐，比如你在Costco和Apple Store附近，即便你之前没有安装过这些应用。   
	
### 2、App实现了handoff功能
*  **[handoff简介](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/Handoff/HandoffFundamentals/HandoffFundamentals.html)：**
	* `OS X 10.10 Yosemite` 新增了一个酷炫的功能 “Hand Off”，打开这个功能之后，用户可以在 Mac 上对 iPad 和 iPhone 进行操作，比如能够编写 iPhone 上未完成的邮件，并且可以在Mac上打开 iPhone 的热点等等， Mac 的 Hand Off 功能只能识别 Mac 周围的 iPhone 手机。      
* **handoff有几个要求：**
	*  1 两台设备都要登录同一个 iCloud 账号。
	*  2 两台设备上的app有相同的 TeamID 。
	*  3 锁屏（或dock）设备上的app支持的 `NSUserActivityTypes` 包含活动设备上的app当前的UserActivityType。
	   	     
### 3、App内有iBeacon信号接收功能，App被iBeacon信号唤醒
* **[iBeacon简介](https://developer.apple.com/ibeacon/)**：
	* 是苹果公司2013年9月发布的移动设备用OS（iOS7）上配备的新功能。工作原理类似之前的蓝牙技术，由 `iBeacon` 发射信号，iOS设备定位接受，反馈信号。根据这项简单的定位技术可以做出许多的相应技术应用,如：`室内定位` 、`商品推荐` 、`微信摇一摇` 等。
* **App icon出现的原因**：
	* `iBeacon` 具备后台定位的能力，只要用户把蓝牙(4.0或以后)开启 和 允许 App 访问位置信息。在有被 App 检测的 `iBeacon` 出现时，如果设备是锁屏状态，设备的左下角就会出现该 App 的 icon 。



参考链接:    
[Make app appear as iOS 8 Suggested App at lockscreen](http://stackoverflow.com/questions/26082414/make-app-appear-as-ios-8-suggested-app-at-lockscreen/26676020#26676020)    
[Can I get my iOS app to appear on the lower left corner of the lock screen?](http://stackoverflow.com/questions/25897219/can-i-get-my-ios-app-to-appear-on-the-lower-left-corner-of-the-lock-screen/25898890#25898890)  
[为什么 iOS 8 锁屏界面的左下角经常会出现某个应用的小图标？](https://www.zhihu.com/question/26653964)    
[关于 IOS8 锁屏左下方出现的 APP ICON](https://www.v2ex.com/t/142320) 

<br>

转载请注明：[潘柏信的博客](http://baixin) » [点击阅读原文](http://baixin.io/2016/09/iOSLowerLeftAppicon/)     
