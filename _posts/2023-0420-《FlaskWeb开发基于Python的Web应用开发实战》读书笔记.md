---
layout: post
title:  "《FlaskWeb开发基于Python的Web应用开发实战》读书笔记"
date:   2023-04-20
description: "《FlaskWeb开发基于Python的Web应用开发实战》读书笔记"
tag: Python
---

## 什么是Flask

Flask是一个基于Python的轻量级Web应用框架，它有两个主要依赖：路由、调试和Web服务器网关接口（Web Server Gateway Interface，WSGI）子系统由Werkzeug提供，模板引擎则由Jinja2提供。

Flask并不原生支持数据库，但是它提供了一个扩展，可以使用Flask-SQLAlchemy来访问数据库。

## Flask的安装

虚拟环境使用第三方实现工具virtualenv创建，输入以下命令查看系统是否安装了virtualenv：

```bash
virtualenv venv
```

在Ubuntu系统中，如果没有安装virtualenv，可以使用如下命令安装：

```bash
sudo apt-get install virtualenv
```

在windows系统中，如果没有安装virtualenv，可以使用如下命令安装：

```bash
pip install virtualenv

pip install virtualenvwrapper  # 这是对virtualenv的封装版本，一定要在virtualenv后安装 
```

下载本书代码，并回退到第一章的代码版本：

```bash
git clone https://github.com/miguelgrinberg/flasky.git
cd flasky
git checkout 1a
```

如果出现git timeout，可以使用如下命令：

```bash
git clone https://gitclone.com/github.com/miguelgrinberg/flasky.git
```

接着使用virtualenv在flasky文件夹中创建虚拟环境：（比如说把虚拟环境命名为venv，会在flasky文件夹中创建一个venv文件夹）

```bash
virtualenv venv
```

激活虚拟环境：

Ubuntu系统：

```bash
source venv/bin/activate
```
Windows系统：

```bash
venv\Scripts\activate
```

虚拟环境被激活之后，命令行前面会出现一个(venv)的前缀，表示当前处于虚拟环境中。

当虚拟环境中的工作完成之后，想回到全局Python解释器中，可以在命令行提示符下输入deactivate命令。

## 程序基本结构

### 初始化

Web服务器使用一种名为Web服务器网关接口（Web Server Gateway Interface，WSGI）的协议，把接收自客户端的所有请求都转交给这个对象处理。程序实例就是一个WSGI应用程序，它的构造函数是Flask类的一个实例。

```python
from flask import Flask
app = Flask(__name__)
```

这个name变量是Python预定义的变量，它的值是模块的名字。在这个例子中，这个值是程序包中的__init__.py文件，因为这个文件是程序包的入口。

### 路由和视图函数

客户端把请求发给Web服务器，Web服务器再把请求发给Flask程序实例。程序实例需要知道对每个URL请求运行哪些代码，所以保存了一个URL到Python的映射关系。处理URL和函数之间的程序称为路由。

```python
@app.route('/') # 装饰器，把修饰的函数注册为路由,这个路由是程序的根路由
def index():
    return '<h1>Hello World!</h1>'
```

修饰器可以使用不同的方法修饰函数的行为，惯常方法是使用修饰器把函数注册为事件的处理程序。

像index()这样的函数被称为视图函数(view function)，视图函数的返回值可以是包含HTM;的简单字符串，也可以是复杂的表单。

直接右键运行程序，flask不启动，原因是没有配置环境变量，需要在命令行中运行程序：

```bash
export FLASK_APP=hello.py
export FLASK_ENV=development
flask run
# flask run -h ipaddress -p port
```

在windows系统中，没有export命令，可以使用如下命令：

```bash
set FLASK_APP=hello.py
set FLASK_ENV=development
flask run
```

还是不行，试一试这个：

```bash
$env:FLASK_APP="hello.py"
$env:FLASK_ENV="development"
flask run
```


在pycharm中可以直接配置configuration来运行。

![20230420171028](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230420171028.png)


hello.py文件中的代码如下：

```python
from flask import Flask
app = Flask(__name__)


@app.route('/')
def index():
    return '<h1>Hello World!</h1>'


@app.route('/user/<name>')
def user(name):
    return '<h1>Hello, {}!</h1>'.format(name)
```

地址输入http://127.0.0.1:5000/user/hugo，可以看到页面显示Hello, hugo!

地址输入http://127.0.0.1:5000，可以看到页面显示Hello World!