---
layout: post
title: "TensorFlow学习笔记"
date: 2016-07-01 
description: "TensorFlow学习笔记"
tag: 机器学习  

---


官方资源：[英文官网](http://tensorflow.org)、[中文官网](http://www.tensorfly.cn/)，[Github项目](https://github.com/tensorflow/tensorflow)

### 简介：
　　TensorFlow是一个用来编写和执行机器学习算法的工具。计算在数据流图中完成，图中的节点进行数学运算，边界是在各个节点中交换的张量（Tensors--多维数组）。TensorFlow负责在不同的设备、内核以及线程上异步地执行代码。
　　TensorFlow在台式机、服务器或者移动设备的CPU和GPU上运行，也可以使用Docker容器部署到云环境中。
　　　　　　![](http://www.tensorfly.cn/images/tensors_flowing.gif)    

<!--more-->

### 用途：
　　`语音识别`，`自然语言理解`，`计算机视觉`，`广告`等等。但是，但我们也不能过分夸大TensorFlow这种通用深度学习框架在一个工业界机器学习系统里的作用。在一个完整的工业界语音识别系统里，除了深度学习算法外，还有很多工作是专业领域相关的算法，以及海量数据收集和工程系统架构的搭建。

### 特性
**1 灵活性**    
　　TensorFlow不是一个严格的神经网络工具包，只要你可以使用数据流图来描述你的计算过程，你可以使用TensorFlow做任何事情。你还可以方便地根据需要来构建数据流图，用简单的Python语言来实现高层次的功能。

**2 可移植性**    
　　TensorFlow可以在任意具备CPU或者GPU的设备上运行，你可以专注于实现你的想法，而不用去考虑硬件环境问题，你甚至可以利用Docker技术来实现相关的云服务。

**3 提高开发效率**    
　　TensorFlow可以提升你所研究的东西产品化的效率，并且可以方便与同行们共享代码。

**4 支持语言选项**     
　　目前TensorFlow支持Python和C++语言。（但是你可以自己编写喜爱语言的SWIG接口）

**5 充分利用硬件资源，最大化计算性能**



### TensorFlow编译iOS的静态库
Tensorflow在0.9版开始支持iOS平台的。按Tensorflow[官方文档的命令](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/makefile)：
1、clone项目   

> $ git clone https://github.com/tensorflow/tensorflow    

2、安装依赖环境，cd 到 tensorflow/contrib/makefile/        

> $ bash download_dependencies.sh       
    
3、安装protobufs    
    
> $ bash compile_ios_protobuf.sh 

4、安装iOS环境      

> make -f tensorflow/contrib/makefile/Makefile \
>  TARGET=IOS \
>  IOS_ARCH=ARM64
      
5、安装图库      


> mkdir -p ~/graphs
> curl -o ~/graphs/inception.zip \
>  https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip \
>  && unzip ~/graphs/inception.zip -d ~/graphs/inception

6、安装iOS最终环境，生成libtensorflow-core.a    

> $ bash build_all_ios.sh

7、如果你的机子沸腾起来了，并且看到了下面的图，代表你已经正式在安装了。
![]()

8、完成编译静态库


#### iOS上遇到问题

1、googlecode.com被墙了，需要翻墙！（目前测试挂了VPN也没用）       

> curl: (7) Failed to connect to googlemock.googlecode.com port 443: Operation timed out

> 解决：        
> 　　1）把下载好的 protobuf 拷贝出来，protobuf在 tensorflow/contrib/makefile/downloads里。    

> 　　2）访问 https://googlemock.googlecode.com/files/gmock-1.7.0.zip，把gmock-1.7.0.zip下载下来放在protobuf/ 里面。    

> 　　3）把tensorflow/contrib/makefile/download_dependencies.sh 里的 git clone https://github.com/google/protobuf.git ${DOWNLOADS_DIR}/protobuf 给注释掉。    

> 　　4）再次执行命令 bash build_all_ios.sh`，立即把之前拷贝的 protobuf 文件放进tensorflow/contrib/makefile/downloads里。    

> 　　5）静静的等待。。。    

2、没有安装 autoconf 和 libtool

>  Google Mock not present.  Fetching gmock-1.7.0 from the web...
>  + autoreconf -f -i -Wall,no-obsolete
> Can't exec "glibtoolize": No such file or directory at /usr/local/Cellar/autoconf/2.69/share/autoconf/Autom4te/FileUtils.pm line 345, <GEN10> line 6.
> autoreconf: failed to run glibtoolize: No such file or directory
> autoreconf: glibtoolize is needed because this package uses Libtool


解决：执行命令   

> brew install libtool
> brew install autoconf
      
我之前安装过了 automake ，如果你没有的话，也是需要安装的。


3、应用程序里没有xcode，或没有一个叫xcode名字的。       

> xcrun: error: SDK "iphoneos" cannot be located    
> xcrun: error: SDK "iphoneos" cannot be located   
> xcrun: error: unable to lookup item 'PlatformPath' in SDK 'iphoneos'   
> + IPHONEOS_PLATFORM=    

4、你的Xcode版本不是7.3或以后，或者你有多个Xcode，而默认的安装路径版本不是7.3或以后。    

     
> error: Xcode 7.3.0 or later is required.      
> + exit 1   


解决：更新Xcode版本到7.3以后，修改为默认安装路径。
如果Xcode是7.3，你可以修改tensorflow/contrib/makefile/compile_ios_tensorflow.sh 里的REQUIRED_XCODE_VERSION=7.3.0，为REQUIRED_XCODE_VERSION=7.3。（这样修改，目前还不确定会不会带来一些其他影响，最好是升级你的Xcode）

#### 参考连接：
[编译Protobuf(On Mac)](http://bafeimao.net/2015/11/14/compile-protobuf-on-mac/)     
[TensorFlow学习笔记1：入门](http://www.jeyzhang.com/tensorflow-learning-notes.html)     
[TensorFlow学习笔记2：构建CNN模型](http://www.jeyzhang.com/tensorflow-learning-notes-2.html)   
[TensorFlow学习笔记3：词向量](http://www.jeyzhang.com/tensorflow-learning-notes-3.html)    
[Caffe、TensorFlow、MXnet三个开源库对比](http://chenrudan.github.io/blog/2015/11/18/comparethreeopenlib.html)   
[如何评价Tensorflow和其它深度学习系统](http://weibo.com/p/1001603907610737775666)    
[深度学习框架大战正在进行，谁将夺取“深度学习工业标准”的荣耀？](http://www.algorithmdog.com/%E8%B0%81%E5%B0%86%E5%A4%BA%E5%8F%96%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E5%B7%A5%E4%B8%9A%E6%A0%87%E5%87%86%E7%9A%84%E8%8D%A3%E8%80%80)  
