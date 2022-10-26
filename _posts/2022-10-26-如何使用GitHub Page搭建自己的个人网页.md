---
layout: post
title: "如何使用GitHub Page搭建自己的个人网页"
date: 2022-10-26 
description: "介绍自己在搭建个人网站时候进行的尝试"
tag: 个人网站 Jekyll GithubPage
---   
### 简介

一般来说，个人主页是自己的一张名片，有些人用来记录自己的生活，有些人用来分享技术、总结自己踩过的坑，有些人用来展示实验室和研究生招生等等。其实很多类似博客园之类的都可以写文章，没有必要自己弄自己的网站。

但是如果想选择自己搭建自己的网站，想选择不一样的样式，那么不妨可以多看看大佬们的主页。

对此，我们需要满足两个条件：

第一，我们可能需要掌握一些前端的编程知识，至少我们需要学会HTML超链接文本的书写，在里面可以展示我们网站想要展示给别人的基本内容；如果需要对网页内容进行美化，则还需要学习CSS（层叠样式表）的基本知识；如果进一步还想和观众进行互动的话，可能还要学习JavaScript的知识，这个其实就跟画PPT一样，我们肯定能学会，但是能不能画好，就是另外一回事了。

第二个，就是要从一些运营商购买域名，比如说我同学haris就有自己的域名，还买了上市运营商的股票。但是主要问题是有一定学习门槛，而且我心疼钱呀呜呜呜！！！

那么，有没有一种方法，能够做到零成本、维护简单、以及不需要额外设计呢？答案是肯定的，GitHub 为我们提供了相应的 Pages 服务。这意味着，只要我们拿到别人已经做好的样式文件（别人写好了css,js等文件，拿来用就行了），再放入自己的 Markdown 文件，就能够建立简易的静态网页，从而实现零成本搭建个人主页的需求。

以下是一些我参考过的，对我研究生生涯很有帮助的个人主页：

1. [炸鸡人博客](https://zhajiman.github.io/)
   1. Python系列
   2. Cartopy系列
   3. Matplotlib 系列
   4. ...
2. [摸鱼的大佬](https://yxy-biubiubiu.github.io/)
3. [Eddy's World](https://iloveocean.top/)
   1. Academic
   2. Life 
      1. [关于抑郁症这件事](https://iloveocean.top/index.php/archives/850/)
      2. [关于自律的一个月尝试](https://iloveocean.top/index.php/archives/555/)
      3. [小白入门系列之一：健身增肌训练](https://iloveocean.top/index.php/archives/318/)
   3. Software
4. [抽筋儿的话语权](https://xuchi.name/)
5. [Clarmy吱声](http://clarmy.net/)

### Jekyll简介

我们首先来解决第一个问题，那就是不会一些前端语言，只会最简单的Markdown能不能写出自己的静态网站？

有滴，套模板就好了。

常用的静态网站生成器有：Jekyll，Hugo，Hexo等。Jekyll使用Ruby语言构建，是GitHub Pages默认支持的生成器；Hugo使用Go语言构建，Hexo使用NodeJS技术构建。据说Hugo速度是最快的,Hexo不同版本配置会差不少。

为什么最后会选择Jekyll生成器呢？
因为我看上的模板是这样搭建的

### 如何使用GitHub Page

GitHubPages 是 GitHub推出的免费静态网页托管服务，我们可以使用 GitHub 或者 Gitee 托管博客、项目官网等静态网页。它们支持以 Jekyll、Hugo、Hexo 等工具编译而成的静态资源。同时网上也存在着较多的 Jekyll Themes、Hexo Themes 、Hugo Themes 等主题资源。对于轻量的博客需求，我们只需拷贝任意一种自己喜欢的主题，然后上传至自己的仓库，就能够通过 "name.github.io" 或者 "name.gitee.io" 等域名访问自己的博客页面。

Gitee据说也可以，但是好像很容易被吞图传

当然这个不是必须的，WordPress等其他网站或者自己搞一个自己的域名也是很好的选择。但是，谁让它是免费的呢；而且还有这么多技术博客教你怎么？

使用github page创建自己的免费个人博客主要包括以下步骤，见[官网步骤](https://pages.github.com/)：

1. 创建一个repository，这个repository一定要命名为username.github.io的格式。
2. 在本地克隆该repository。
3. 进入该项目文件夹并进行修改，可以多找找其他人的博客，确定真的喜欢才行。我第一次找了个二次元大佬的博客主页，后来发现不少很适合自己。
4. 将修改发布到repository：Add, commit, and push your changes。
5. 刷新一下，应该一两分钟之后就可以在自己的网站看到自己的更改。
6. 或者可以启动本地服务，现预览一遍可能的效果：jekyll serve然后在Server address: http://127.0.0.1:4000查看效果。

### 撰写博文的过程

#### 比如说从写一个Markdown简介开始

#### 写完之后你的项目文件夹应该是怎么样的呢？

这是我写完博客之后的文件夹列表(有删减)
```
sources
│  .gitignore
│  404.html
│  about.md
│  archive.html
│  feed.xml
│  Gemfile
│  Gemfile.lock
│  index.html
│  Rakefile
│  README.md
│  support.md
│  tags.html
│  _config.yml
│  
├─.jekyll-cache
│  └─Jekyll
│      └─Cache
│          ├─Jekyll--Cache
│          │  └─b7
│          │          9606fb3afea5bd1609ed40b622142f1c98125abcfe89a76a661b0e8e343910
│          │          
│          └─Jekyll--Converters--Markdown                     
├─code
│  │  2022-07-11-D-Tale库介绍.ipynb
│  │  
│  └─.ipynb_checkpoints
│          2022-07-11-D-Tale库介绍-checkpoint.ipynb
│          
├─css
│      animate.css
│      main.css
│      post.css
│      tomorrow.css     
├─images
│  │  avatar.jpg
│  │  background-cover.jpg
│              
├─js
│      highlight.pack.js
│      main.js
│      
├─_includes
│  │  comments.html
│  │  external.html
│  │  footer.html
│  │  head.html
│  │  new-old.html
│  │  pagination.html
│  │  read-more.html
│  │  side-panel.html
│  │  toc.html
│  │  
│  ├─JB
│  │      pages_list
│  │      posts_collate
│  │      setup
│  │      tags_list
│  │      
│  └─styles
│          main.css
│          
├─_layouts
│      default.html
│      page.html
│      post.html
│      
├─_posts
│      2022-07-10-D-Tale库介绍.md
│      2022-07-10-markdown简介.md
│      2022-08-09-海表面高度数据下载.md
│      2022-10-26-前端入门.md
│      2022-10-27-如何使用GitHub Page搭建自己的个人网页.md
│      
└─_site
    │  404.html
    │  feed.xml
    │  index.html
    │  Rakefile
    │  README.md
    │  
    ├─2022
    │  ├─07
    │  │  ├─D-Tale库介绍
    │  │  │      index.html
    │  │  │      
    │  │  └─markdown简介
    │  │          index.html
    │  │          
    │  └─08
    │      └─海表面高度数据下载
    │              index.html
    │              
    ├─about
    │      index.html
    │      
    ├─archive
    │      index.html
    │      
    ├─code
    │      2022-07-11-D-Tale库介绍.ipynb
    │      
    ├─css
    │      animate.css
    │      main.css
    │      post.css
    │      tomorrow.css
    │      
    ├─images
    ├─js
    │      highlight.pack.js
    │      main.js
    │      
    ├─support
    │      index.html
    │      
    └─tags
            index.html
            
```


注意事项：
1. 发布时间：有一个凌晨，我文章写好了，也push上去了，也没发现错误，但是左等右等都不见个人主页更新这篇文章。后来才知道，github page应该托管的服务器在美国，用的是美国东部时间可能，也就是说你在今天发布的内容，至少要到中午才能看到。
   

### 待解决功能

1. 评论功能，我之前按照教程去弄，但是好像是个韩国的网站，它给我发了注册信息，但是都被我邮箱垃圾桶照单全收了。两个月之后，我才在垃圾箱里面找到，但是那个时候我已经不知道怎么搞了，真的是“出师未捷身先死。”

### 参考连接

1. [Jekyll搭建个人博客](http://leopardpan.github.io/2016/10/jekyll_tutorials1/) (但是这个大佬好像以及不更新了)
2. [用 Hugo 重新搭建博客](https://zhajiman.github.io/post/rebuild_blog/)
3. 