---
layout: post
title:  "Jekyll+github搭建个人博客"
categories: jekyll update
tag: jekyll
---
# 新建Repository
  进入个人github主页，创建一个Repository。
  ![creat_a_new_repository.png](photo/creat_a_new_repository.png)
  切记，博客Repository的名字一定是***.github.io(个人github名+.github.io)，这样才能直接通过[***.github.io](spigzhu.github.io)的域名访问个人博客
# 开启Github Pages
  进入Repository设置界面，进行博客Repository设定，包括选择博客主题。
  ![set_github_pages.png](photo/set_github_pages.png)
# 安装 &配置Jekyll
jekyll官网文档[Jekyll安装文档](https://jekyllrb.com/docs/)有各种操作系统的安装教程，直接指示操作即可。
## 安装Jekyll之前需要先安装Ruby
Ruby是一种简单快捷的面向对象的脚本语言，而RubyGems是Ruby的一个包管理器，我们需要用这个来安装Jekyl。
如果是unix系统的话，安装比较简单。我这里用到的是ubantu系统，安装教程如下：[ubuntu系统安装Ruby](https://jekyllrb.com/docs/installation/ubuntu/)。
```
sudo apt-get install ruby-full build-essential zlib1g-dev
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```
## 安装Jekyll
```
gem install jekyll bundler
```
# 通过jekyll管理自己的博客网站
参考jekyll官网教程：[jekyll管理博客文件](https://jekyllrb.com/docs/#instructions)
## 创建个人博客文件目录
```
jekyll new myblog
```
## 进入博客文件目录
```
cd myblog
```
## 在本地服务器上运行个人博客网站
```
bundle exec jekyll serve
```
jekyll处于运行状态时，会生成一个Server address：[http:/127.0.0.1:4000/](http:/127.0.0.1:4000/)，打开这个链接就可以看到jekyll的页面。Jekyll刚安装好后，只有一篇名为“Welcome to Jekyll!”的博文。
## 创建新博文
在myblog/_posts目录下新建一个markdown文件，在markdown文件前面添加以下front matter:
```
---
layout: post
title:  "Welcome to Jekyll!"
date:   2021-01-03 20:28:08 +0800
categories: jekyll update
---
```
front matter中layout为post表示这是一篇博文，title date categories分别表示标题/时间/类别。
# 将jekyll创建的博文发布到个人博客上
* 将刚才创建的github博客repo拷贝到本地
* 在myblogm目录的_config.yml文件把baseurl改成repo的名字
* 修改为config需要执行‘bundle exec jekyll serve’，build jekyll网站。
* 把myblogw目录下所有文件拷贝到repo目录下
* 把repo刚才添加的所有文件都push到github上

支持，通过Jekyll+github搭建个人博客就大功告成啦～
