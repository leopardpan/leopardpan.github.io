---
layout: post
title: iOS开发迎来机器学习的春天---TensorFlow 
date: 2016-07-07 
tags: 机器学习    
---

<div align="center">
	<img src="/images/posts/tfimg/logo.jpg" height="300" width="500">  
</div> 

　　`人工智能`、`机器学习`都已走进了我们的日常，尤其是愈演愈热的大数据更是跟我们的生活息息相关，做 `人工智能`、`数据挖掘`的人在其他人眼中感觉是很高大上的，总有一种遥不可及的感觉，在我司也经常会听到数据科学部的同事们提到 `机器学习`、`数据挖掘` 之类的词。但这些名词真的跟我们移动开发就没直接关系了吗？             
　　作为移动开发者来说，无时无刻不被这些名词狠狠地敲打着脆弱的内心。💢 💢 💢  何时才能够将`机器学习`、`深度学习`应用在移动端，敲响移动端`机器学习`工业化的大门呢？

> 想象一下，某一天你身处一个完全陌生的环境，周围都是陌生的事物，而运行在iPhone的某个APP却对这个环境了如指掌，你要做的就是打开这个APP，输入你需要了解的事物，iPhone告诉你这个事物的信息，你也就没有了陌生事物了。世界就在眼前！

如下图：
<div align="center">
	<img src="/images/posts/tfimg/image02.png" height="300" width="480" />
</div> 

上面物体的识别准确率还是蛮不错的，基本识别出了键盘（49%的概率）、鼠标（46%的概率）和水杯（24%的概率）。

但是在某些事物的识别准确度方便却差强人意，比如下图：    

<div align="center">
　　<img src="/images/posts/tfimg/image01.png" height="300" width="320" />
</div> 
　　iPhone 6被识别成了iPod（59%的概率），而iPod的却是不怎么敢认（10%的概率）。想想最崩溃的估计是iPhone 6了，身价直接被降了好几个等级。

<div align="center">
　　<img src="/images/posts/tfimg/wq.jpg" height="320" width="240" />  
</div> 

　　上面的例子来自于TensorFlow官方iOSDemo，暂且不评述TensorFlow的识别准确度如何，毕竟它还年轻，但是仅凭其识别能力的体现，也给机器学习在移动端的运用带来了无限的可能。

### 一、TensorFlow（简称TF）

　　去年，Google资深系统专家Jeff Dean在湾区机器学习大会上隆重介绍了其第二代深度学习系统[TensorFlow](http://www.tensorflow.org/)，一时间网络上针对TensorFlow的文章铺天盖地，[揭秘TensorFlow：Google开源到底开的是什么？](http://www.leiphone.com/news/201511/UDLyNds2oSTwM2yZ.html)、[Google开源TensorFlow系统，这背后都有什么门道？](http://www.leiphone.com/news/201511/Voza1pFNQB4bzKdR.html)、[如何评价Google发布的第二代深度学习系统TensorFlow?](http://www.zhihu.com/question/37243838)等等文章，TensorFlow的燎原之火一直在燃烧蔓延着，其[GitHub上的开源库](https://github.com/tensorflow/tensorflow)在此文撰写时，也已经被`star：27550`，`fork：11054`了。🔥 🔥 🔥 🔥 🔥 

不负众望，Google一直宣称平台移植性非常好的TensorFlow，终于在2016年6月27日，发布0.9版本，宣布移动端支持。[TensorFlow v0.9 now available with improved mobile support](https://developers.googleblog.com/2016/06/tensorflow-v09-now-available-with.html)( 有墙💢 )，同时也给出了移动端的[Demo](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/ios_examples)，对于代码为生的程序员，身处大数据处理为主导的[TalkingData](http://www.talkingdata.com/)，也小试身手了一把，下载TensorFlow源码，查看编译指南，开始跳坑、填坑之路，也成就了此篇拙文的产生。


### 二、从TensorFlow到iOS静态库

对于iOS平台下如何使用TensorFlow，TensorFlow给出了详细的编译脚本命令，详情请查看[官方文档的命令](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/makefile)。

##### 第一步. 工具准备

`工欲善其事必先利其器`，在开始编译工作之前，需要准备一些编译所必须的工具：

1.  [Homebrew](http://brew.sh/): Mac os x 上包管理工具，具体使用方法可参考[Doc](http://brew.sh/index_zh-cn.html)。

```
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

2. Homebrew安装好之后，依次安装三个辅助性编译工具：

```
$ brew install libtool   
$ brew install autoconf   
$ brew install automake   
```

> 三个工具的含义，请参考：[https://en.wikipedia.org/wiki/GNU_Libtool](https://en.wikipedia.org/wiki/GNU_Libtool)


##### 第二步. 克隆TensorFlow

Google以[Apache 2.0](http://www.apache.org/licenses/LICENSE-2.0)开源协议将TensorFlow开源在[GitHub](https://github.com/tensorflow/tensorflow)上，我们可以直接使用TensorFlow源码。

在任意你想存放TensorFlow源码的地方（建议不要放在桌面。^_^），clone项目。

```
$ git clone https://github.com/tensorflow/tensorflow 
```

##### 第三步. 编译前准备 

　　在TensorFlow的`tensorflow/contrib/makefile/`目录下，有很多可使用的编译脚本，其中`build_all_ios.sh`脚本专门用来一键编译TensorFlow iOS静态库。虽然可以直接使用此脚本进行一键编译，但是因为有墙，某些依赖需要提前做处理。

1. 下载protobuf

    protobuf 是编译前唯一需要特殊处理的依赖库，[点击下载](https://github.com/google/protobuf/archive/master.zip)，下载protobuf之后，解压，备用。


2. 下载googlemock

    虽然protobuf编译脚本`autogen.sh`中的googlemock链接地址`https://googlemock.googlecode.com/files/gmock-1.7.0.zip`无法直接下载到，但是细心的人会发现，在浏览器中输入`https://googlemock.googlecode.com/`地址后，会跳转到`https://github.com/google/googlemock`地址，google在GiHub上的仓库地址。而GitHub上的仓库，我们可以直接的下载，克隆等。

    我们直接在GitHub上下载googlemock([点击下载](https://github.com/google/googlemock/archive/master.zip))，下载完成后，修改压缩包名字为`gmock-1.7.0.zip`，修改后将此压缩包移至上一步protobuf文件夹目录下，备用。

3. 修改下载依赖脚本，移除protobuf的下载

	在`tensorflow/contrib/makefile/`目录下，`download_dependencies.sh`脚本用来下载相关依赖，打开此脚本文件，注释掉或者直接删掉`git clone https://github.com/google/protobuf.git ${DOWNLOADS_DIR}/protobuf`部分，目的是不让脚本去下载protobuf。

	上面三步准备好后，接下来就进入静态库编译了。

##### 第四步. 一键编译  

　　前面已经知道在TensorFlow文件夹`tensorflow/contrib/makefile/`目录下的`build_all_ios.sh`脚本是用来编译iOS静态库的脚本，因此可以直接执行此脚本，开始静态库的编译工作了。

　　但是有一个问题大家可能会发现，由于编译TensorFlow需要用到protobuf，但是protobuf使我们自己手动下载的，该怎么让手动下载的protobuf能够直接让`build_all_ios.sh`脚本使用呢？

　　答案是`复制、粘贴`。可能有些low，但是有效。执行命令 `build_all_ios.sh`之后，立即把之前手动下载的protobuf文件夹拷贝进`tensorflow/contrib/makefile/downloads`目录。（放心，你拷贝的速度会很快，不会影响编译的执行的。^_^） 

```
$ build_all_ios.sh    
```

　　一切准备就绪，接下来就是静静的等待编译完成了。在Mac编译的过程中，建议插上电源，最好不要让设备休眠断电，也最好不要去干别的东西，出去溜达一圈，回来后就看到战果了。

 编译完成之后，会在`tensorflow/contrib/makefile/gen/`目录下看到编译的结果，关于这些静态库该如何使用，自己的项目如何应用，请参考[TensorFlow iOS Examples](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/ios_examples)。


### 三、遇到的问题

1、googlecode.com被墙了，需要翻墙！（目前测试挂了VPN也没用），这也是上面编译前准备为什么要那么做的原因。

```
curl: (7) Failed to connect to googlemock.googlecode.com port 443: Operation timed out
```

解决： 请参考 『第三步. 编译前准备』。

2、没有Xcode。

```
xcrun: error: SDK "iphoneos" cannot be located
xcrun: error: SDK "iphoneos" cannot be located
xcrun: error: unable to lookup item 'PlatformPath' in SDK 'iphoneos'
+ IPHONEOS_PLATFORM=
```

解决：安装Xcode，从上面报错的命令中可以看到，在编译静态库的过程中使用了`xcrun`，而此命令是xCode本身具有的能力。

3、你的Xcode版本不是7.3或以后，或者你有多个Xcode，而默认的安装路径版本不是7.3或以后。

```
error: Xcode 7.3.0 or later is required.
+ exit 1
```/

解决：更新Xcode至最新版本，并且保证默认路径下是最新/版本。

如果Xcode是7.3，并且没有条件更新Xcode，你可以修改`tensorflow/contrib/makefile/compile_ios_tensorflow.sh` 里的`REQUIRED_XCODE_VERSION=7.3.0`，为`REQUIRED_XCODE_VERSION=7.3`。（这样修改，目前还不确定会不会带来一些其他影响，最好是升级你的Xcode）


### 四、参考链接 
 

* [TensorFlow 中文社区](http://tensorfly.cn/)
* [TensorFlow for Mobile](https://www.tensorflow.org/mobile.html)
* [Caffe、TensorFlow、MXnet三个开源库对比](http://chenrudan.github.io/blog/2015/11/18/comparethreeopenlib.html)   
* [如何评价Tensorflow和其它深度学习系统](http://weibo.com/p/1001603907610737775666)    
* [深度学习框架大战正在进行，谁将夺取“深度学习工业标准”的荣耀？](http://www.algorithmdog.com/%E8%B0%81%E5%B0%86%E5%A4%BA%E5%8F%96%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%B7%A5%E4%B8%9A%E6%A0%87%E5%87%86%E7%9A%84%E8%8D%A3%E8%80%80)  

<br>

转载请注明：[潘柏信的博客](http://baixin) » [点击阅读原文](http://baixin.io/2016/07/iOSMachineLearning_TensorFlow/)        
