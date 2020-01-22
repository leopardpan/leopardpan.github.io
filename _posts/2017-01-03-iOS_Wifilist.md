---
layout: post
title: Wifi 定位原理及 iOS Wifi 列表获取
date: 2017-01-03 
tag: iOS
---

　　对于大家来说，Wifi 应该是一个很熟悉的词了，我们每天都可能在使用 Wifi 热点。Wifi 除了能给我们提供热点之外同时还有定位的作用， 现在移动设备的对用户的隐私保护是越来越严格了，就如定位功能，必须要经过设备用户的授权才能使用 Location 给这台设备定位。这些严格的隐私政策对用户起到到保护作用，但对开发人员却是一种阻碍，在产品强需求的情况下用户是会授权的，如地图类应用，但是另外一些没有对定位强需求的产品，用户可能就不会给你授权了，这是我们可以考虑下 Wifi 定位了。

### Wifi 定位原理

　　当我们使用手机扫面 Wifi 的时候，其实就可以定位到这台手机的位置信息了。每个 Wifi 路由开启后，都会不停的往四周发射信号，我们把 Wifi 路由想象成太阳以某种频度不停的往周围发射电磁波，电磁波会因距离的削弱，同时也会因为物体阻挡而削弱。例子就是我们在离 Wifi 路由器同样远的位置，有些地方信号强度高有些地方信号强度低。路由同时也叫 Wifi 热点（或者 Wifi AP：Access Point）。每一个 Wifi 路由器都会有一个 BSSID，很多人都管这个 BSSID 叫 MAC 地址（其实 BSSID 并不是 MAC 地址），BSSID 设定了一般就不会在变也不会重复，也就意味着是全球唯一的，这是路由制造的规则，既然有规则那么就会有不遵守规则的人，文章结尾会介绍不遵守规则的人是如何害人害己的。

　　刚才提到的 BSSID，在 Wifi 路由器的发射中是可以检测到的，同时 Wifi 路由信号还伴随着，SSID(路由器的名称：如XX的Wifi)、signalStrength（手机接收到Wifi的信号强度）及其它信息。看到这里你应该知道如何使用Wifi定位的了，条件：唯一不变的BSSID 和 手机到路由器的信号强度。思路：Wifi 信号是有范围的，我们假设这个范围就是10米为半径的一个圆(实际情况根据Wifi路由厂商和路由器周围环境而定)，我们去采集一些Wifi热点回来，某家水果店的 Wifi、某家餐馆的 Wifi 等等，我们自己去采集的我们肯定知道他们的具体位置，及刚才提到的 Wifi 中的信息：BSSID、SSID、signalStrength，再把他们存入数据库，采集的人可以很多：专业采集人员、出租车司机、快递员等等，他们经常穿梭于大街小巷，其实我们每个人都是Wifi数据库的采集人员，我们的手机厂商每天都在默默的采集着我们的位置信息，iPhone手机系统设置里就可以看到你今天去哪了，你的Wifi连接过哪些设备也是知道的。时间越久Wifi数据库信息越丰富，最终会发现每个BSSID会对应多个SSID和signalStrength，因为SSID是可以修改的，signalStrength是由于在这个Wifi热点的周围不同位置采集的，所以信号强度也不同。采集的信号强度越多，给BSSID也就是这个Wifi热点的定位就越精准。

　　现在如果我去一个陌生的地方，我打开手机扫描周围的 Wifi 刚好扫描到了一个或几个，我把这个 Wifi 信息（BSSID）传给服务器，服务器通过这个 BSSID 去数据库查找，就能直接匹配到对应的位置，返回给我。如果匹配不到则表示这里没人来采集过 Wifi 信息，或者是这个 Wifi 热点是最近布置的，采集人员还没来得及采集。服务器可以把这些未采集到的先分类后期统一规划。

　　Wifi 定位整体功能是需要服务端来配合的，也就表示必须要有网络环境才行。其实移动端(手机、Pad等)也可以独立完成，不过对技术和设备硬件要求会高很多，全球的 Wifi 热点是一个很庞大的数据量，需要经过高精度的无损压缩后放在内存很大的手机里才行，或许多年以后可以实现吧(即使技术上能实现了，对于产品和研发来说收益、风险、和工作量又是一场PK)


### iOS 申请获取 Wifi 列表权限

　　知道了原理有啥用呢，能实现么？好吧现在就遇到问题了，移动设备如今主要是 Andorid 和 iOS, Android 上可以直接扫描 Wifi 列表获取相关信息，自己去网上找找, 所以说会原理不一定会技术实现，我也就只能讲讲 iOS 的技术实现了。                
　　iOS 上获取 Wifi 列表其实也有很大限制，在 iOS 9 以前是不能获取Wifi列表的，只能获取当前连接的 Wifi 信息，也就表示只有连接了 Wifi 才能定位，刚才文章说到的场景是，我在一个陌生的原理，拿出手机扫描 Wifi ，也就是我并没连接那里的 Wifi（我不知道密码我怎么连啊）。Apple 在 iOS 9 以后，提供了获取Wifi列表的API，但是获取Wifi列表是有门槛的，主要步骤有：

>* 1、向 Apple 申请开发 Network Extension 权限
>* 2、申请包含 Network Extension 的描述文件
>* 3、配置 Info.plist 
>* 4、配置 entitlements
>* 5、iOS 获取 Wifi 列表代码实现
>* 6、获取Wifi列表回调

### 1、向 Apple 申请开发 Network Extension 权限

　　首先要先写封邮件给 [networkextension@apple.com](mailto:networkextension@apple.com) ，问苹果要开发 Network Extension 的权限。     
苹果收到邮件后会自动回复邮件，在 [https://developer.apple.com/contact/network-extension/](https://developer.apple.com/contact/network-extension/) 里面填写申请表格，内容包括：     

```
Organization：               

Company / Product URL:             

What's your product's target market?              

What's your company's primary function?             

Describe your application and how it will use the Network Extension framework.            

What type of entitlement are you requesting?                     

。。。
```

申请后大概两周左右能收到 Aplle的 确认信，如：

```
Hi, 

Thanks for your interest in the Network Extension APIs.

We added a new template containing the Network Extension entitlements to your team.

。。。。
```

### 2、申请包含 Network Extension 的描述文件

![](/images/posts/Wifilist/PastedGraphic.png)

选择包含 Network Extension 的描述文件，后点击下载，下载完成双击描述文件。

### 3、配置 Info.plist 

Xcode Info.plist 里 Required background modes 添加 一个 network-authentication(item)

![](/images/posts/Wifilist/infoplist.png)

### 4、配置 entitlements

Demo.entitlements（Demo是项目名称） 里添加 Key-Value: com.apple.developer.networking.HotspotHelper -> YES

![](/images/posts/Wifilist/entitlement.png)

### 5、iOS 获取 Wifi 列表代码实现

导入头文件

```
#import <NetworkExtension/NetworkExtension.h>  
```

代码实现

```                
- (void)getWifiList {

	if (![[[UIDevice currentDevice] systemVersion] floatValue] >= 9.0) {return;}
	dispatch_queue_t queue = dispatch_queue_create("com.leopardpan.HotspotHelper", 0);
	[NEHotspotHelper registerWithOptions:nil queue:queue handler: ^(NEHotspotHelperCommand * cmd) {
		if(cmd.commandType == kNEHotspotHelperCommandTypeFilterScanList) {
			for (NEHotspotNetwork* network  in cmd.networkList) {
				NSLog(@"network.SSID = %@",network.SSID);
			}
		}
	}];
}
```
kNEHotspotHelperCommandTypeFilterScanList： 表示扫描到 Wifi 列表信息。

NEHotspotNetwork 里有如下信息：

>* SSID：Wifi 名称 
>* BSSID：站点的 MAC 地址
>* signalStrength： Wifi信号强度，该值在0.0-1.0之间     
>* secure：网络是否安全 (不需要密码的 Wifi，该值为 false)
>* autoJoined： 设备是否自动连接该 Wifi，目前测试自动连接以前连过的 Wifi 的也为 false 。
>* justJoined：网络是否刚刚加入
>* chosenHelper：HotspotHelper是否为网络的所选助手

[官方文档连接](https://developer.apple.com/reference/networkextension/nehotspotnetwork)


### 6、获取Wifi列表回调

当你把上面的代码写完，并成功运行项目后，发现并没有Wifi列表的回调。因为你还没刷新Wifi列表，你需要：

* 打开手机系统设置 -> WLAN -> 系统 Wifi 列表加载出来时，上面代码部分才会回调，才能获取到 Wifi 列表。

<img src="/images/posts/Wifilist/WLAN.png" height="360" width="200">  

这个时候你就能看到控制台源源不断的Log。

### 注意事项

* 1、获取Wifi列表功能由于是需要申请后台权限，所以能后台激活App(应用程序)，而且激活后App的进程能存活几个小时。
* 2、整个获取Wifi列表不需要App用户授权，也就是在App用户无感知下获取设备的Wifi列表信息，使用时请正当使用。
* 3、Wifi列表获取 NetworkExtension 是 iOS 9以后才出的，目前 iOS 9 已经覆盖很广了。


下面付一张来自 [TalkingData 对iOS操作系统的统计报表](https://www.talkingdata.com/index/#/device/os/zh_CN)，时间：2017-01-03

<img src="/images/posts/Wifilist/systemVersion.png" height="280" width="600">  

### Q&A

在操作过程或者文章有问题的话欢迎在 [原文](http://baixin.io/2017/01/iOS_Wifilist/) 里提问或指正。

>* 使用 Demo 我就不提供了，你如果没有申请 NetworkExtension 权限，提供了 Demo 你也无法使用。

<br>

参考资源：[NEHotspotHelper NetworkExtension API iOS9.0](http://stackoverflow.com/questions/31704292/nehotspothelper-networkextension-api-ios9-0)

<br>
转载请注明：[潘柏信的博客](http://baixin) » [Wifi 定位原理及 iOS Wifi 列表获取](http://baixin.io/2017/01/iOS_Wifilist/)  


