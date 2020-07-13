---
layout: post
title: 文档支持的Markdown语法
date: 2016-11-20 
tags: markdown    
---


## 什么是 Markdown

Markdown 是一种方便记忆、书写的纯文本标记语言，用户可以使用这些标记符号以最小的输入代价生成极富表现力的文档：如您正在阅读的这篇文章。它使用简单的符号标记不同的标题，分割不同的段落，**粗体** 或者 *斜体* 某些文字.

很多产品的文档也是用markdown编写的，并且以“README.MD”的文件名保存在软件的目录下面。
　　
## 一些基本语法

标题            
H1 :# Header 1            
H2 :## Header 2           
H3 :### Header 3           
H4 :#### Header 4           
H5 :##### Header 5            
H6 :###### Header 6      
链接 :[Title](URL)        
加粗 :**Bold**        
斜体字 :*Italics*         
*删除线 :~~text~~          
内嵌代码 : `alert('Hello World');`        

### 列表

* 列表1
* 列表2
* 列表3

### 列表引用

>* 列表1
>* 列表2
>* 列表3

### 插入一张图片

打赏一个吧

![](/images/payimg/weipayimg.jpg)

css 的大部分语法同样可以在 markdown 上使用，但不同的渲染器渲染出来的 markdown 内容样式也不一样，下面这些链接里面有 markdown 基本语法，你也可以在下面几个平台上尝试着写一些。

## 博客支持的高级语法

### 1. 制作一份待办事宜 

- [ ] 支持以 PDF 格式导出文稿
- [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
- [x] 新增 Todo 列表功能
- [x] 修复 图标功能

### 2. 高亮一段代码

```python

@requires_authorization
class SomeClass:
    pass

if __name__ == '__main__':
    # A comment
    print 'hello world'

```

#### 3. 绘制表格

| 项目        | 价格   |  数量  |
| --------   | -----:  | :----:  |
| 计算机     | \$1600 |   5     |
| 手机        |   \$12   |   12   |
| 管线        |    \$1    |  234  |


