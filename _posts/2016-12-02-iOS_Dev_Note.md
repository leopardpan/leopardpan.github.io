---
layout: post
title: iOS开发中的小问题记录
date: 2016-12-02 
tag: iOS
---

### NSKeyedArchiver 自定义对象写文件


如果存储的对象类名有变动，则需要设置clasName, 方法为：“setClassName:forClass:”        
使用 NSKeyedArchiver 进行数据持久化时, 系统会默认使用类名去建表，如果类名变了，那么使用新的类名肯定是从本地获取不到表的，代码执行崩溃。     
所以需要在 NSKeyedArchiver 或者 NSKeyedUnarchiver 时使用 “setClassName:forClass:” 指定类名。 


### 断点配置：【Generate Debug Symbols】     

描述: 用来控制断点是否生效,关闭此功能，打包 `.ipa` 时，包体积会小很多。    
配置路径:【project/TARGETS/Build Settings/Apple LLVM7.1 - Code Genneration/Generate Debug Symbols】    


### 捕获全局异常：【All Exception】    

描述: 用来捕捉整个项目在 Xcode 里执行时的异常。例如：try/catch 时 catch住的异常,【All Exception】可以直接定位到具体位置。     
配置路径: 异常捕捉(commod+7)/Xcode左下角点击+/Add Exception Breakpoint/完成(回车键)  


### UI相关

1、设置状态栏颜色：

```

info.plist 添加 View controller-based status bar appearance - NO     
代码里写 [[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleLightContent]; 再次运行后状态栏就会变成白色。    

```

2、左滑返回手势失效了怎么办：   

```    

设置 navigationItem.leftBarButtonItem 之后，左滑返回手势就会失效。设置一下 UIGestureRecognizerDelegate 代理即可：

self.navigationController.interactivePopGestureRecognizer.delegate = self;

```

3、让 TableView的 下拉 和 上拉 显示不一样的背景颜色：

```

给 TableView 上加一个 View，View 的 Frema：
CGRectMake(0, -self.view.bounds.size.height, self.view.bounds.size.width, self.view.bounds.size.height + 2)，
给变View的背景颜色就可以了。

```


<br>
转载请注明：[潘柏信的博客](http://baixin) » [iOS开发中的小问题记录](http://baixin.io/2016/12/iOS_Dev_Note/)  


