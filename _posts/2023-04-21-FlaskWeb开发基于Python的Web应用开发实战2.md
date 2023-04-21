---
layout: post
title: "FlaskWeb开发基于Python的Web应用开发实战2"
date: 2023-04-21
description: "FlaskWeb开发基于Python的Web应用开发实战2"
tag: Python
---

## 2.5 请求-响应循环

### 2.5.1 程序和请求上下文

Flask从客户端收到请求时，要让视图函数能访问一些对象，这样才能处理请求。Flask把这些对象封装在一个叫做请求上下文（request context）的对象中，这个对象在请求被处理前被激活，处理完成后被自动销毁。Flask使用上下文全局变量（context global variable）**g**来存储处理请求期间可能用到的临时变量。这些变量在请求结束后自动清理。

举例子：

```python
from flask import request
@app.route('/')
def index():
    user_agent = request.headers.get('User-Agent')
    return '<p>Your browser is %s</p>' % user_agent
```

在这个视图函数中，我们把request当做全局变量使用。事实上，request不可能是全局变量。多个线程在处理不同客户端发送的不通气请求时，每个线程看到的request对象必然不同。Flask使用上下文让特定的变量在一个线程中全局可访问，与此同时缺不会干扰其他线程。


（多线程Web服务器会创建一个线程池，再从线程池中取出一个线程来处理请求。）

程序在上下文被推送后，可以在线程中使用current_app和g变量。current_app是程序实例本身，g是一个空对象，用于存储请求期间的临时变量。请求上下文被推送之后，就可以使用request和session变量了。

以下演示了程序上下文的使用方法：


![20230421083730](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230421083730.png)


没有激活程序上下文之前就调用current_app.name会报错，但推送完上下文后就可以调用了。（在程序实例上调用app.app_context()方法，返回一个程序上下文对象。）


### 2.5.2 请求调度

程序收到客户端发来的请求时，要找到处理该请求的视图函数。Flask使用URL映射（URL map）来实现这一功能。URL映射是一个把URL和视图函数关联起来的字典。Flask在请求被处理前，会把URL映射表保存在程序上下文中。这样，当请求到来时，就可以通过URL映射表找到对应的视图函数。

Flask使用app.route修饰器或者非修饰器形式的app.add_url_rule()函数来生成URL映射。

检查为hello.py程序生成的URL映射表：

```shell
(venv) $ python
>>> from hello import app
>>> app.url_map
Map([<Rule '/static/<filename>' (OPTIONS, GET, HEAD) -> static>,
 <Rule '/' (OPTIONS, GET, HEAD) -> index>,
 <Rule '/user/<name>' (OPTIONS, GET, HEAD) -> user>])
```

发现有三条Rule,分别是'/static/<filename>'、'/'和'/user/<name>'。这三条规则分别对应着三个视图函数。其中，'/'规则对应着index视图函数，'/user/<name>'路由是Flask添加的特殊路由，用于访问静态文件。

URL映射中的HEAD/Options/GET是请求方法，由路由器处理。Flask为每个路由都指定了请求方法，这样不同的请求方法发送到相同的URL上时，可以调用不同的视图函数。

### 2.5.3 请求钩子

为了避免在每个视图函数中都重复编写相同的代码，Flask提供了请求钩子（request hook）机制。请求钩子是在请求被分发到视图函数之前调用的通用函数。

Flask支持以下4种钩子。

- before_first_request：注册一个函数，在处理第一个请求之前运行。
- before_request：注册一个函数，在每次请求之前运行。
- after_request：注册一个函数，如果没有未处理的异常抛出，在每次请求之后运行。
- teardown_request：注册一个函数，即使有未处理的异常抛出，也在每次请求之后运行。

请求钩子函数和视图函数之间共享数据一般使用上下文全局变量g。例如，before_request钩子可以从数据库中加载已登录用户，然后把用户对象保存在g.user中，这样视图函数就可以使用g.user获取用户。

### 2.5.4 响应

Flask会把视图函数返回的值转换为响应对象，然后把响应对象发送给客户端。

视图函数返回的值可以是一个包含响应体、状态码和响应头的元组。

有一种名为重定向的特殊响应，它的状态码是302，表示客户端应该重定向到其他位置。重定向的目标位置在响应头的Location字段中指定。

## 2.6 Flask扩展

扩展是Flask的一个重要组成部分。扩展是对Flask功能的补充，可以为Flask程序添加新的功能。Flask扩展通常以Flask-为前缀，例如Flask-SQLAlchemy。

安装Flask-Script扩展：

```shell
(venv) $ pip install flask-script
```

runserver命令可以启动开发服务器：

```shell
(venv) $ python hello.py runserver
```

其中，--host和--port选项分别指定服务器监听的地址和端口。默认情况下，服务器只监听本机的5000端口。如果想让Web服务器监听公共网络接口上的连接，允许同网中的其他计算机连接服务器，可以使用--host 0.0.0.0选项。

那个，Web服务器就可以使用http://a.b.c.d:500/网络中的任何以太电脑进行访问，其中a.b.c.d是服务器所在计算机的外网IP地址。



#### 版本问题

Flask以及Jinja2，werkzeug版本过高过低都不行：

- Werkzeug 0.16.1
- Flask 1.1.2
- Jinja2 3.0.2




# 第三章 模板


视图函数的作用很明确，也即是生成请求的响应；但是一般来说，请求会改变程序的状态，而这种变化也会在视图函数中产生。

例如，用户在网站中注册了一个新账户，用户在表单中输入电子邮箱地址和密码，然后点击提交按钮。服务器接收到用户输入数据的请求，然后Flask把请求分发到注册请求的视图函数，这个视图函数需要访问数据库，添加新用户，然后生成响应回送浏览器。这两个过程分别成为业务逻辑和表现逻辑。

把业务逻辑和表现逻辑混在一起，这就会导致视图函数的代码量变得很大，而且难以维护。把表现逻辑移动到模板中，可以让视图函数专注于业务逻辑，这样视图函数的代码量就会变小，而且更容易维护。



模板是一个包含响应文本的文件，其中包含了一些特殊的标记，这些标记会在模板被渲染时被替换为动态内容。Flask使用Jinja2模板引擎来渲染模板。

比较常见的模板标记有：

- boostrap, 前端框架, 用于快速搭建网站
- moment.js, 用于在浏览器中处理日期和时间

## 第四章 Web表单

有些任务很单调，而且要重复操作，比如说生成表单的HTML代码和验证提交的表单数据。

Flask-WTF扩展封装了WTForms，并且和Flask框架集成在一起，可以使用Flask-WTF快速实现常见的Web表单功能。

### 4.1 跨站请求伪造保护

## 第五章 数据库

基于关系模型的数据库，这种数据库也被称为SQL数据库，因为他们使用结构化查询语言。

最近几年文档数据库和键值对数据库成为了流体的选择，这两种数据库合称为NoSQL数据库。



