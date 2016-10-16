---
layout: post
title: "Jekyll搭建个人博客"
date: 2016-10-14 
tags: 博客   
---

　　之前写了一篇[HEXO搭建个人博客](http://baixin.io/2015/08/HEXO%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)的教程获得了很好评，尤其是在[简书](http://www.jianshu.com/p/465830080ea9)上面，目前已经累积了10W+的阅读量了，也有好心的读者主动给我打赏，在此感谢。

　　如果你看过我的文章【HEXO搭建个人博客】会发现我现在的博客样式跟之前是有很大的区别的，之前我也是使用HEXO搭建的博客，后来发现使用HEXO在多台电脑上发布博客，操作起来并不是那么方便，果断的就转到了Jekyll上了，接下来我会讲如何使用Jekyll搭建博客，[博客模板效果](http://baixin.io/#blog)。

### Jekyll环境配置

 使用Jekyll搭建博客之前要确认下本机环境，Git环境（用于部署到远端）、Ruby环境（Jekyll是基于Ruby开发的）。
[Jekyll中文文档](http://jekyll.bootcss.com/)、[Jekyll英文文档](https://jekyllrb.com/)、[Jekyll主题列表](http://jekyllthemes.org/)


安装jekyll

```     
$ gem install jekyll     
```    

创建博客

```    
$ jekyll new myBlog    
```   

进入博客目录

```
$ cd myBlog  
```

启动本地服务
```
$ jekyll serve
```

在浏览器里输入： [http://localhost:4000](http://localhost:4000)，就可以看到你的博客效果了。

![](/assets/images/jekyll/image1.png)

so easy !

我们来看看myBlog里面有哪些文件

```
myBlog　　　　　　　　　　　
　　｜　　　　　　
　　｜－－_config.yml　　　　　　　　　
　　｜－－_includes　　　　　　　　
　　｜－－_layouts　　　　　　　　　　
　　｜－－_posts　　　　　　　
　　｜－－_sass　　　　　　　　
　　｜－－_site　　　　　　　　　　
　　｜－－about.md　　　　　　　
　　｜－－css　　　　　　　
　　｜－－feed.xml　　　　　　　　
　　｜－－index.html　　　　　　　
　　｜　
```


进入_config.yml里面，修改成你想看到的信息，重新 `jekyll server` ，刷新浏览器就可以看到你刚刚修改的信息了。

到此，博客初步搭建算是完成了，

### 博客部署到远端 

　　我这里讲的是部署到Github Page(Coding Page也可以)创建一个github账号，然后创建一个跟你账户名一样的仓库，如我的github账户名叫[leopardpan](https://github.com/leopardpan)，我的github仓库名就叫[leopardpan.github.io](https://github.com/leopardpan/leopardpan.github.io)，创建好了之后，把刚才建立的myBlog项目push到username.github.io仓库里去（username指的是你的github用户名），检查你远端仓库已经跟你本地myBlog同步了，然后你在浏览器里输入username.github.io，就可以访问你的博客了。


### 使用我的博客模板

虽然博客部署完成了，你会发现博客太简单不是你想要的，如果你喜欢我的模板的话，可以使用我的模板。

首先你要获取的我博客，[Github项目地址](https://github.com/leopardpan/leopardpan.github.io.git)，你可以直接[点击下载博客](https://github.com/leopardpan/leopardpan.github.io/archive/master.zip)，进去leopardpan.github.io/ 目录下， 使用命令部署本地服务 

```
$ jekyll server   
```

在浏览器输入 [127.0.0.1:4000](127.0.0.1:4000) ， 就可以看到[baixin.io](http://baixin.io)博客效果了。

### 修改成你自己的博客

>* 如果你想使用我的模板请把_posts/ 目录下的文章都去掉。
>* 修改_config.yml文件里面的内容为你自己的。

然后使用git push到你自己的仓库里面去，检查你远端仓库，在浏览器输入username.github.io就会发现，你有一个漂亮的主题模板了。      

### 为什么要是用Jekyll

使用了Jekyll你会发现如果你想使用多台电脑发博客都很方便，只要把远端github仓库里的博客clone下来，写文章后再提交就可以了，Hexo由于远端提交的是静态网页，所有无法直接写Markdown的文章。如果你想看Hexo搭建博客，可以看看我的另一篇[HEXO搭建个人博客](http://baixin.io/2015/08/HEXO%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)的教程。

如果你在搭建博客遇到问题，可以在[原文博客](http://baixin.io/2016/06/jekyll_tutorials1/)的评论里给我提问。










