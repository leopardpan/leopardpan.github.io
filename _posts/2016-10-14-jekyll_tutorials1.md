---
layout: post
title: Jekyll搭建个人博客
date: 2016-10-14 
tags: jekyll   
---

　之前写了一篇[HEXO搭建个人博客](http://leopardpan.cn/2015/08/HEXO%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)的教程获得了很好评，尤其是在[简书](http://www.jianshu.com/p/465830080ea9)上目前已经累积了10W+的阅读量了，也有好心的读者主动给我打赏，在此感谢。

　如果你看过我的文章会发现我现在的博客样式跟之前是有很大的区别的，之前我也是使用 HEXO 搭建的博客，后来发现使用 HEXO 在多台电脑上发布博客，操作起来并不是那么方便，果断就转到了 Jekyll 上，接下来我会讲如何使用 Jekyll 搭建博客，[博客模板效果](http://leopardpan.cn/#blog)。


### 介绍

 　Jekyll 是一个简单的博客形态的静态站点生产机器。它有一个模版目录，其中包含原始文本格式的文档，通过 Markdown （或者 Textile） 以及 Liquid 转化成一个完整的可发布的静态网站，你可以发布在任何你喜爱的服务器上。Jekyll 也可以运行在 GitHub Page 上，也就是说，你可以使用 GitHub 的服务来搭建你的项目页面、博客或者网站，而且是完全免费的

　使用 Jekyll 搭建博客之前要确认下本机环境，Git 环境（用于部署到远端）、[Ruby](http://www.ruby-lang.org/en/downloads/) 环境（Jekyll 是基于 Ruby 开发的）、包管理器 [RubyGems](http://rubygems.org/pages/download)                
　　如果你是 Mac 用户，你就需要安装 Xcode 和 Command-Line Tools了。下载方式 Preferences → Downloads → Components。

　　Jekyll 是一个免费的简单静态网页生成工具，可以配合第三方服务例如： Disqus（评论）、多说(评论) 以及分享 等等扩展功能，Jekyll 可以直接部署在 Github（国外） 或 Coding（国内） 上，可以绑定自己的域名。[Jekyll中文文档](http://jekyll.bootcss.com/)、[Jekyll英文文档](https://jekyllrb.com/)、[Jekyll主题列表](http://jekyllthemes.org/)。


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

![](/images/posts/jekyll/image1.png)

so easy !

### 目录结构
　
　Jekyll 的核心其实是一个文本转换引擎。它的概念其实就是： 你用你最喜欢的标记语言来写文章，可以是 Markdown，也可以是 Textile,或者就是简单的 HTML, 然后 Jekyll 就会帮你套入一个或一系列的布局中。在整个过程中你可以设置URL路径, 你的文本在布局中的显示样式等等。这些都可以通过纯文本编辑来实现，最终生成的静态页面就是你的成品了。

 一个基本的 Jekyll 网站的目录结构一般是像这样的：

```
.
├── _config.yml
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   ├── post.html
|   └── page.html
├── _posts
|   └── 2016-10-08-welcome-to-jekyll.markdown
├── _sass
|   ├── _base.scss
|   ├── _layout.scss
|   └── _syntax-highlighting.scss
├── about.md
├── css
|   └── main.scss
├── feed.xml
└── index.html

```

这些目录结构以及具体的作用可以参考 [官网文档](http://jekyll.com.cn/docs/structure/) 

进入 _config.yml 里面，修改成你想看到的信息，重新 jekyll server ，刷新浏览器就可以看到你刚刚修改的信息了。

到此，博客初步搭建算是完成了，

### 博客部署到远端 

　我这里讲的是部署到 Github Page 创建一个 github 账号，然后创建一个跟你账户名一样的仓库，如我的 github 账户名叫 [leopardpan](https://github.com/leopardpan)，我的 github 仓库名就叫 [leopardpan.github.io](https://github.com/leopardpan/leopardpan.github.io)，创建好了之后，把刚才建立的 myBlog 项目 push 到 username.github.io仓库里去（username指的是你的github用户名），检查你远端仓库已经跟你本地 myBlog 同步了，然后你在浏览器里输入 username.github.io ，就可以访问你的博客了。


### 编写文章

　　所有的文章都是 _posts 目录下面，文章格式为 mardown 格式，文章文件名可以是 .mardown 或者 .md。

　　编写一篇新文章很简单，你可以直接从 _posts/ 目录下复制一份出来 `2016-10-16-welcome-to-jekyll副本.markdown` ，修改名字为 2016-10-16-article1.markdown ，注意：文章名的格式前面必须为 2016-10-16- ，日期可以修改，但必须为 年-月-日- 格式，后面的 article1 是整个文章的连接 URL，如果文章名为中文，那么文章的连接URL就会变成这样的：http://leopardpan.cn/2015/08/%E6%90%AD%E5/ ， 所以建议文章名最好是英文的或者阿拉伯数字。 双击 2016-10-16-article1.markdown 打开

```

---
layout: post
title:  "Welcome to Jekyll!"
date:   2016-10-16 11:29:08 +0800
categories: jekyll update
---

正文...

```


title: 显示的文章名， 如：title: 我的第一篇文章                    
date:  显示的文章发布日期，如：date: 2016-10-16                          
categories: tag标签的分类，如：categories: 随笔            

注意：文章头部格式必须为上面的，.... 就是文章的正文内容。

我写文章使用的是 Sublime Text2 编辑器，如果你对 markdown 语法不熟悉的话，可以看看[作业部落的教程](https://www.zybuluo.com/) 


### 使用我的博客模板

虽然博客部署完成了，你会发现博客太简单不是你想要的，如果你喜欢我的模板的话，可以使用我的模板。

首先你要获取的我博客，[Github项目地址](https://github.com/leopardpan/leopardpan.github.io.git)，你可以直接[点击下载博客](https://github.com/leopardpan/leopardpan.github.io/archive/master.zip)，进去leopardpan.github.io/ 目录下， 使用命令部署本地服务 

```
$ jekyll server   
```

### 如果你本机没配置过任何jekyll的环境，可能会报错

```
/Users/xxxxxxxx/.rvm/rubies/ruby-2.2.2/lib/ruby/site_ruby/2.2.0/rubygems/core_ext/kernel_require.rb:54:in `require': cannot load such file -- bundler (LoadError)
	from /Users/xxxxxxxx/.rvm/rubies/ruby-2.2.2/lib/ruby/site_ruby/2.2.0/rubygems/core_ext/kernel_require.rb:54:in `require'
	from /Users/xxxxxxxx/.rvm/gems/ruby-2.2.2/gems/jekyll-3.3.0/lib/jekyll/plugin_manager.rb:34:in `require_from_bundler'
	from /Users/xxxxxxxx/.rvm/gems/ruby-2.2.2/gems/jekyll-3.3.0/exe/jekyll:9:in `<top (required)>'
	from /Users/xxxxxxxx/.rvm/gems/ruby-2.2.2/bin/jekyll:23:in `load'
	from /Users/xxxxxxxx/.rvm/gems/ruby-2.2.2/bin/jekyll:23:in `<main>'
	from /Users/xxxxxxxx/.rvm/gems/ruby-2.2.2/bin/ruby_executable_hooks:15:in `eval'
	from /Users/xxxxxxxx/.rvm/gems/ruby-2.2.2/bin/ruby_executable_hooks:15:in `<main>'

```

原因： 没有安装 bundler ，执行安装 bundler 命令

```

$ gem install bundler

```


提示： 

```
Fetching: bundler-1.13.5.gem (100%)
Successfully installed bundler-1.13.5
Parsing documentation for bundler-1.13.5
Installing ri documentation for bundler-1.13.5
Done installing documentation for bundler after 5 seconds
1 gem installed

```

再次执行 $ jekyll server  ，提示

```

Could not find proper version of jekyll (3.1.1) in any of the sources
Run `bundle install` to install missing gems.

```

跟着提示运行命令

```
$ bundle install
```

这个时候你可能会发现 bundle install 运行卡主不动了。

如果很长时间都没任何提示的话，你可以尝试修改 gem 的 source

```
$ gem sources --remove https://rubygems.org/
$ gem sources -a http://ruby.taobao.org/
$ gem sources -l
*** CURRENT SOURCES ***

http://ruby.taobao.org

```

再次执行命令 $ bundle install，发现开始有动静了

```
Fetching gem metadata from https://rubygems.org/...........
Fetching version metadata from https://rubygems.org/..
Fetching dependency metadata from https://rubygems.org/.
。。。
Installing jekyll-watch 1.3.1
Installing jekyll 3.1.1
Bundle complete! 3 Gemfile dependencies, 17 gems now installed.
Use `bundle show [gemname]` to see where a bundled gem is installed.

```

bundler安装完成，后再次启动本地服务 

```
$ jekyll server

```

继续报错

```
Configuration file: /Users/tendcloud-Caroline/Desktop/leopardpan.github.io/_config.yml
  Dependency Error: Yikes! It looks like you don't have jekyll-sitemap or one of its dependencies installed. In order to use Jekyll as currently configured, you'll need to install this gem. The full error message from Ruby is: 'cannot load such file -- jekyll-sitemap' If you run into trouble, you can find helpful resources at http://jekyllrb.com/help/! 
jekyll 3.1.1 | Error:  jekyll-sitemap

```
表示 当前的 jekyll 版本是 3.1.1 ，无法使用 jekyll-sitemap 

解决方法有两个

> 1、打开当前目录下的 _config.yml 文件，把 gems: [jekyll-paginate,jekyll-sitemap] 换成 gems: [jekyll-paginate] ，也就是去掉jekyll-sitemap。

> 2、升级 jekyll 版本，我当前的是 jekyll 3.1.2 。

修改完成后保存配置，再次执行

```
$ jekyll server

```
提示

```
Configuration file: /Users/baixinpan/Desktop/OpenSource/Mine/Page-Blog/leopardpan.github.io-github/_config.yml
            Source: /Users/baixinpan/Desktop/OpenSource/Mine/Page-Blog/leopardpan.github.io-github
       Destination: /Users/baixinpan/Desktop/OpenSource/Mine/Page-Blog/leopardpan.github.io-github/_site
 Incremental build: disabled. Enable with --incremental
      Generating... 
                    done in 0.901 seconds.
 Auto-regeneration: enabled for '/Users/baixinpan/Desktop/OpenSource/Mine/Page-Blog/leopardpan.github.io-github'
Configuration file: /Users/baixinpan/Desktop/OpenSource/Mine/Page-Blog/leopardpan.github.io-github/_config.yml
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.

```

表示本地服务部署成功。

在浏览器输入 [127.0.0.1:4000](127.0.0.1:4000) ， 就可以看到[leopardpan.cn](http://leopardpan.cn)博客效果了。

### 修改成你自己的博客

>* 如果你想使用我的模板请把 _posts/ 目录下的文章都去掉。
>* 修改 _config.yml 文件里面的内容为你自己的。

然后使用 git push 到你自己的仓库里面去，检查你远端仓库，在浏览器输入 username.github.io 就会发现，你有一个漂亮的主题模板了。      


### 为什么要是用 Jekyll

使用了 Jekyll 你会发现如果你想使用多台电脑发博客都很方便，只要把远端 github 仓库里的博客 clone 下来，写文章后再提交就可以了，Hexo 由于远端提交的是静态网页，所有无法直接写 Markdown 的文章。如果你想看 Hexo 搭建博客，可以看看我的另一篇[HEXO搭建个人博客](http://leopardpan.cn/2015/08/HEXO%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)的教程。

如果你在搭建博客遇到问题，可以在[原文博客](http://leopardpan.cn/2016/10/jekyll_tutorials1/)的评论里给我提问。

后面会继续介绍，在我的博客基础上，如何修改成你自己喜欢的 Style，欢迎继续关注我博客的更新。


### Q&A 

> 问题：最近很多朋友使用我的模板报警告：The CNAME `leopardpan.cn` is already taken 
> 解决：把CNAME里面的baixin.io修改成你自己的域名，如果你暂时没有域名，CNAME里面就什么都不用谢。（之前没人反馈过这个问题，应该是github page最近才最的限制。）









