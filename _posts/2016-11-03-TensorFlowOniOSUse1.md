---
layout: post
title: TensorFlow 在 iOS 平台上的使用(一)
date: 2016-11-03 
tags: 机器学习    
---

　　距离上次使用 TensorFlow 在iOS平台上做的小 Demo，已经过了四个月了，今天忽然想再看看,发现 Demo 已经不见了，我只能从头在编一次，这次发现编译 iOS 库，简单多了。

　　tensorflow [下载地址](https://github.com/tensorflow/tensorflow/archive/master.zip)，tensorflow 最近提交的时间：2016-11-03，commit：`7b7c02de56e013482b5fe5ab05e576dc98fe5742` 。


　　下载完成后打开文件，找到目录 `tensorflow-master/tensorflow/contrib/ios_examples` 你会发现目录下有三个项目和一个 README.md 。

```
benchmark 、 camera 、 simple 、README.md

```

### 如果你发现项目无法运行，请看这里

　　对于任何项目我们首先打开的应该是 README.md ，里面一般情况都会有介绍如何使用这个项目，tensorflow 也不会例外。README 开头就说了，这个目录里有如何在 iOS 平台上使用 tensorflow 的例子，但是需要注意几点：

* 你的 Xcode 版本必须是 7.3 或更高版本，并且有安装 command-line 工具 。
* 项目(Examples) 里必须包含一个静态库：`libtensorflow-core.a` 。
* 下载 [Inception v1](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip)，解压后将 label 和 graph 放在 simple 和 camera 的项目中。

### camera 项目的使用

　　camera 项目在 tensorflow-master/tensorflow/contrib/ios_examples 目录下，如果你是直接打开 camera 项目，编译你会发现报错缺少 imagenet_comp_graph_label_strings.txt 和 tensorflow_inception_graph.pb 两个文件，这两个文件上面已经说到了下载 [Inception v1](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip) 解压得到。现在还差 静态库：`libtensorflow-core.a` ，这个需要我们自己编译。

#### 编译静态库：libtensorflow-core.a

进入目录：tensorflow-master/tensorflow/contrib/makefile，你可以看到一大堆 .sh 结尾的文件，找到 build_all_ios.sh ，Mac 上可以直接在 termina（终端）上运行命令编译

```
$ sh build_all_ios.sh
```

这个编译的过程是很漫长的，一般在一个小时左右。也有可能你在编译的过程中会遇到问题，这次我只遇到一个问题：

```
configure.ac:30: error: required file 'build-aux/ltmain.sh' not found
configure.ac:24: installing 'build-aux/missing'
Makefile.am: installing 'build-aux/depcomp'
parallel-tests: installing 'build-aux/test-driver'
autoreconf: automake failed with exit status: 1

```

解决方法是：先卸载 libtool 在重新安装，`brew uninstall libtool` && `brew install libtool`

如果你还遇到了其它问题，可以看看我之前的一片文章 [iOS开发迎来机器学习的春天---TensorFlow](http://baixin.io/2016/07/iOSMachineLearning_TensorFlow/) ，或者是直接去 tensorflow 的 [Issues](https://github.com/tensorflow/tensorflow/issues) 里面找。 

一个小时后。。。　如果编译没出问题，你可以在目录　`tensorflow-master/tensorflow/contrib/makefile／gen/lib` 下找到一个静态库：`libtensorflow-core.a` ，把这个静态库拷贝到 camera 项目中，然后编译运行。

<br>
转载请注明：[潘柏信的博客](http://baixin) » [点击阅读原文]()     













