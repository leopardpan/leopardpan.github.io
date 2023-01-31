---
layout: post
title: "Markdowm插件Office Viewer介绍以及vscode+PicGO+Github搭建个人博客图床"
date: 2023-01-31
description: "Markdowm插件Office Viewer介绍以及vscode+PicGO+Github搭建个人博客图床"
tag: 自动化测试
---
Markdowm插件Office Viewer介绍以及vscode+PicGO+Github搭建个人博客图床

## Markdowm插件

* Markdown All in One
* Office Viewer

  这个插件真的太棒了，对于md文件就像开了挂一样的好用。安装完之后，会设置好Vditor，它是一款浏览器端的 Markdown 编辑器，支持所见即所得、即时渲染（类似 Typora）和分屏预览模式。 它使用 TypeScript 实现，支持原生 JavaScript 以及 Vue、React、Angular 和 Svelte 等框架。
  稍微体验了一下，有如下优点：

图片、表格、代码块，列表（有序、无序、任务）、引用、分隔、斜体等傻瓜式操作；方便快速写标题以及大纲视图；导出PDF极其方便；最上方预览小按钮点击之后进入预览界面，可以选择桌面或者手机端浏览，可以把写好的内容直接发布到微信公众号或者知乎；CSDN博客的内容如果是用markdown写的，直接复制进来，格式都不用调整。

![office_viewer使用示意图](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/office_viewer使用示意图.jpg)

## PicGO介绍

vscode上有一个picgo插件，可以让我们用快捷键即可上传图片到默认的免费服务器，具体的使用方法是，安装完成后：

1. Ctrl+Alt+U 从剪切板粘贴图片，上传到服务器；
2. Ctrl+Alt+E 打开资源管理器，选择图片。

picgo默认上传的服务器是 `SM.MS`服务器，由于你懂得原因，国内的访问速度让人抓狂，但是picgo支持github以及阿里云作为图床。

其中，github图床最为稳定，且一个仓库上限是100G，只做图床绰绰有余。最糟糕的一点是速度，但是使用cdn加速之后速度还是可以的。

## Github图床设置

1. 打开github，创建新的public仓库，仓库名称可以设置为[PicgoBed](https://github.com/ChanJeunlam/PicgoBed)。
2. 进入个人设置页面，选择开发者设置

   ![github_setting](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/github_setting.jpg)
3. 选择Personal access tokens，选择生成新token，此token是图床上传时验证身份用的；添加描述，将repo选上，Expiration选择no expiration

   ![tokens](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/tokens.jpg)
4. 将生成的字符串保存，**关闭页面后将再也无法看到这个字符串了**

## PicGO设置

GitHub设置完之后，我们需要修改picgo插件的设置，把服务器编程我们的图床，具体设置如下：

1. vscode右下角选择设置,搜索设置里面输入PicGO,打开扩展里面的PicGO
2. 设置如下,github token写我们刚才复制下来的

 ![picgosetting](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/picgosetting.jpg)

* `current`设置为GitHub
* `Branch`是我们仓库的分支，默认为 `master` 或者 `main`
* `custom url` 使我们图片上传的连接，有两种方式可以使用
  * 原生方式
    * 使用GitHub原生连接，格式为 `https://raw.githubusercontent.com/用户名/仓库名/分支名`
    * 我的例子 `https://raw.githubusercontent.com/ChanJeunlam/PicgoBed`
    * 原生方式有一个弊端就是，国内速度比较慢
  * cdn加速方式
    * 格式为 `https://cdn.jsdelivr.net/gh/用户名/仓库名@分支名`
    * 我的例子 `https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed`
    * cdn加速的优点是国内访问速度比较快
* `path`是我们的图片存储在仓库中的路径，比如我的是 `blogs/pictures/`
* `Repo`是我们的仓库地址，`Username/Repo`,比如说我的是 ``ChanJeunlam/PicgoBed``.

其他可能出现的问题的处理:

* `Branch`里面的main写错成master
* `ChanJeunlam/PicgoBed` 加了空格，写错成 `ChanJeunlam / PicgoBed`，导致上传到服务器失败。
