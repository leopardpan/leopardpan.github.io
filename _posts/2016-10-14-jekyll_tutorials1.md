---
layout: post
title: "Jekyll搭建个人博客"
date: 2016-10-14 
tags: 博客   
---

　　之前写了一篇[HEXO搭建个人博客](http://baixin.io/2015/08/HEXO%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)的教程获得了很好评，尤其是在[简书](http://www.jianshu.com/p/465830080ea9)上面，目前已经累积了10W+的阅读量了，也有好心的读者主动给我打赏，在此感谢。

　　如果你看过我的文章会发现我现在的博客样式跟之前是有很大的区别的，之前我也是使用 HEXO 搭建的博客，后来发现使用 HEXO 在多台电脑上发布博客，操作起来并不是那么方便，果断就转到了 Jekyll 上了，接下来我会讲如何使用 Jekyll 搭建博客，[博客模板效果](http://baixin.io/#blog)。


### 介绍
　　使用 Jekyll 搭建博客之前要确认下本机环境，Git 环境（用于部署到远端）、Ruby 环境（Jekyll 是基于 Ruby 开发的）、管理器 gem。
[Jekyll中文文档](http://jekyll.bootcss.com/)、[Jekyll英文文档](https://jekyllrb.com/)、[Jekyll主题列表](http://jekyllthemes.org/)。
　　Jekyll 是一个免费的简单静态网页生成工具，可以配合第三方服务例如： Disqus（评论）、多说(评论) 以及分享 等扩展功能，不需要数据库支持。并且 Jekyll 可以部署在Github（国外） 或 Coding（国内） 上，可以绑定自己的域名，而且这是完全免费的。

### Jekyll 环境配置

安装 jekyll

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

　　我这里讲的是部署到 Github Page 创建一个 github 账号，然后创建一个跟你账户名一样的仓库，如我的 github 账户名叫 [leopardpan](https://github.com/leopardpan)，我的 github 仓库名就叫 [leopardpan.github.io](https://github.com/leopardpan/leopardpan.github.io)，创建好了之后，把刚才建立的 myBlog 项目 push 到 username.github.io仓库里去（username指的是你的github用户名），检查你远端仓库已经跟你本地 myBlog 同步了，然后你在浏览器里输入 username.github.io ，就可以访问你的博客了。


### 编写文章

　　所有的文章都是 _posts 目录下面，文章格式为 mardown 格式，文章文件名可以是 .mardown 或者 .md。
　　编写一篇新文章很简单，你可以直接从 _posts/ 目录下复制一份出来 `2016-10-16-welcome-to-jekyll副本.markdown` ，修改名字为 2016-10-16-article1.markdown ，注意：文章名的格式前面必须为 2016-10-16- ，日期可以修改，但必须为 年-月-日- 格式，后面的article1是整个文章的连接URL，如果文章名为中文，那么文章的连接URL就会变成这样的：http://baixin.io/2015/08/%E6%90%AD%E5/ ， 所以建议文章名最好是英文的或者阿拉伯数字。 双击 2016-10-16-article1.markdown 打开

```

---
layout: post
title:  "Welcome to Jekyll!"
date:   2016-10-16 11:29:08 +0800
categories: jekyll update
---

....

```


title: 显示的文章名， 如：title: 我的第一篇文章 。
date:  显示的文章发布日期，如：date: 2016-10-16 。
categories: tag标签的分类，如：categories: 随笔 。

注意：文章头部格式必须为上面的，.... 就是文章的正文内容。

我写文章使用的是 Sublime Text2 编辑器，如果你对 markdown 语法不熟悉的话，可以看看[作业部落的教程](https://www.zybuluo.com/) 


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

后面会继续介绍，在我的博客基础上，如何修改成你自己喜欢的Style，欢迎继续关注我博客的更新。








