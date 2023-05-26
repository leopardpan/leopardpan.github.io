---
layout: post
title: Ubuntu安装Docker
date: 2023-05-26
description: "Ubuntu安装Docker"
tag: Linux
---


## 在ubuntu中安装

在linux系统中安装Docker非常简单，官方为我们提供了一键安装脚本。这个方法也适用于Debian或CentOS等发行版。

```
curl -sSL https://get.daocloud.io/docker | sh
```

在线安装其他方法：

```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

-fsSL：-f/--fail：连接失败时不显示http错误信息；-s/--silent：静默模式，不显示进度条或错误信息；-S/--show-error：显示错误信息；-L/--location：跟随重定向。

启动Docker后台服务：

```
systemctl enable docker
#启动Docker服务：
systemctl start docker
```

安装完成后，我们可以查看Docker版本信息：

```
docker version
```

## 安装docker-compose

1. 登入[官方地址](https://github.com/docker/compose/releases)下载指定版本，官方地址为：
https://github.com/docker/compose/releases
下载docker-compose-linux-x86_64

2. 安装并且验证

```shell
mv docker-compose-Linux-x86_64 /usr/local/bin/docker-compose
chmod 777 /usr/local/bin/docker-compose
docker-compose --version
```


## nvm安装对应版本的node

1. 安装nvm



```shell
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash

nvm --version
```

或者从gitee上下载

```shell
git clone https://gitee.com/mirrors/nvm
cd nvm
bash install.sh
```


安装好之后显示如下信息：

```
=> Compressing and cleaning up git repository

=> Appending nvm source string to /home/kan/.bashrc
=> Appending bash_completion source string to /home/kan/.bashrc
=> Close and reopen your terminal to start using nvm or run the following to use it now:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

2. node v12.18.1+npm 6.14.5安装
   
```shell
nvm install 12.18.1
nvm use 12.18.1
node -v
```

查看对应的npm版本

```shell
npm -v
```


3. 卸载nvm

```shell
cd ~
rm -rf .nvm
```



