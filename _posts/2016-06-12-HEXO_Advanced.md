---
layout: post
title: HEXO进阶
tag: 博客
---

HEXO接近是最近有一些朋友提出的问题，然后我做了总结，如果你也在使用HEXO，不妨看看，应该会有些帮助。

* 1、博客部署样式出问题了怎么办？
* 2、电脑重装或者误删了本地博客怎么办？
* 3、想使用两台电脑写博客怎么办？
* 4、为何使用百度搜不到我的博客？


### 使用Jekyll解决前三个问题。
不得不说 `Jekyll` 确实可以解决我上面三个问题, 因为 `Jekyll` 是直接把Markdown格式的文章直接放在github仓库里的, 相当于直接用git来管理博客了, `Github` 官方也很推荐 `Jekyll` 。 你可以先看下 `Jekyll` 搭建博客的[voyagelab](voyagelab.github.io), [github地址](https://github.com/voyagelab/voyagelab.github.io), 当然了这只是很普通的, Jekyll 也有很多主题可以选择的, 更详细的请看[Jekyll中文文档](http://jekyll.bootcss.com/)、[Jekyll英文文档](https://jekyllrb.com/)、[Jekyll主题列表](http://jekyllthemes.org/)。
在 `Jekyll` 上逛了一个星期的我又回到了 `Hexo` , 发现 目前 `Jekyll` 对主题和一些插件的支持相对 `Hexo` 来说, 没那么友好, 可能有一些其它的方法只是我没找到而已,关于 `Jekyll` 搭建博客就介绍到这, 如果有问题的话可以评论, 或者联系我。

### 使用Hexo解决上面前三个问题
是的, 我大`Hexo`同样可以解决上面三个问题, 那就是使用git。关于如何使用 `Hexo` 搭建博客请看我另一篇文章[HEXO搭建个人博客](http://www.baixin.io/2015/08/25/HEXO%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/), 如果搭建的过程中出现了问题, 我们可以交流交流。现在我假设你已经能基本使用 `Hexo` 了, 接下来就看看如何来管理博客。

## 使用git管理博客
[Blog-Growing](https://github.com/leopardpan/Blog-Growing)是我管理博客的仓库地址。结构大致是:

> -- Blog-Growing     
> 　　|-- .git     
> 　　|-- .gitignore    
> 　　|-- Hexo     
> 　　　　|   ..    
> 　　　　|   ..    
> 　　　　|   整个博客的配置信息    

你可以把我的的[Blog-Growing](https://github.com/leopardpan/Blog-Growing) clone下来, 看看一些基本配置是怎么配置的。需要提醒的是, `要把主题下的.git给删除掉` , 当然了你也可以使用 `git submodule` 来管理, 具体的管理方法我这里就不做多的介绍了, 想了解的话可以私信我。
上面的过程基本做完后, 怎么来解决我开篇提的前三个问题了。

### 具体实现:   
**一：家里电脑使用博客**        
　　建立git远端仓库管理博客,并使用家里的电脑把本地博客的配置推送到远端仓库。   
**二：公司电脑使用博客**         
　　到了公司只需要执行`sudo npm install -g hexo`,然后cd到你的博客目录下,如我cd 到Hexo目录下, 然后执行 `hexo server` 就可以在本地预览博客了。    
**三：使用Git保存**          
　　修改好博客后记得先使用git来提交下, 即使下次把博客的样式修改坏了, 也可以使用 `git reset --hard` 来回退。如: 我cd 到 `Blog-Growing` 目录下使用git提交。   
**四：博客提交**           
　　1、修改好的博客使用 `hexo d` 展示到博客页上。   
　　2、git push 整个本地博客。    

**提示:** 在这里 `git` 仅仅只是用户做博客的版本管理的, 博客的样式修改、基本部署还是使用 `hexo` 来操作的。

## 让百度能搜索到你的博客

### 为什么要使用百度搜索？

　　有人可能会说作为一个开发人员, 你不会用 `Google` 啊。 是的, Google是能搜到我们搭建在 `Github Page` 的博客, 会用`Google` 也是一个开发人员必备技能之一。但是, 我们生活在天朝, 所以百度还是总有会用到的时候, 或者是你想让更多的天朝人能搜到你。

### 为什么使用百度搜索不到 Github Page 上的博客？   

有人联系过 Github Support 部门 , 给出大致的意思就是: 百度爬虫爬得太猛烈，已经对很多 Github 用户造成了问题。所以 Github 将禁止百度爬虫的爬取。    

### 如何让百度能搜索你的博客?   

　　根据上面说的, 目前发现只是Github Page禁止了百度搜索, 所以让百度能搜索到你的博客还是有一些方法的。例如:
* 自己搞个VPS,博客部署在VPS上。
* 博客部署 `Coding.net` 上, `GitCafe`已经合并到 `Coding` 。
我使用的是第二种方法, 博客部署在 `Coding.net` 上也相对简单些。

#### 在Coding上部署你的博客。   

　　Coding同样支持Hexo、Jekyll等博客的部署, Coding 跟Github还是挺像的,而且是中文。 同样的在Coding里面建一个项目,项目名字跟你的用户名一样,这里我就不啰嗦了, 说几个需要注意的地方:     
**注意一:**       
　　在`Coding Page` 上部署博客,需要把博客推送到`coding-pages ` 分支上, 分支名字是固定的。    
**注意二:**     
　　`Coding Page` 不支持自定义CNAME, 你需要点击到Page模块,然后添加一个域名来绑定。   

更详细的请看[Coding Pages 官网介绍](https://coding.net/help/doc/pages/index.html).     

参考文章:
[解决 Github Pages 禁止百度爬虫的方法与可行性分析](http://jerryzou.com/posts/feasibility-of-allowing-baiduSpider-for-Github-Pages/)

<br>

转载请注明：[潘柏信的博客](http://baixin) » [点击阅读原文](http://baixin.io/2016/06/HEXO_Advanced/)