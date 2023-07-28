---
layout: post
title: Docker中运行和部署Hugo博客
date: 2023-07-26
description: "Docker中运行和部署Hugo博客"
tag: Docker
---
# 创建Dockerfile

```shell
#使用基础镜像
FROM debian:12.0-slim

# 安装hugo
RUN apt-get update \
    && apt-get install -y hugo \
    && rm -rf /var/lib/apt/lists/*

# 暴露1313端口
EXPOSE 1313/tcp

# entrypoint防止docker 容器运行后退出
ENTRYPOINT ["tail", "-f", "/dev/null"]
```

## 构建镜像

在Dockerfile对应的文件夹⬇️执行 ：

```shell
docker build -t hugotools:1.0 .
```

执行完将通过我们的Dockerfile来构建一个hugotools 1.0版本的镜像!

## 运行容器

```shell
docker run --name hugotool --rm -p 127.0.0.1:1313:1313 -v /home/hugo/hugo-blog-data:/app hugotools:1.0
```

/home/hugo/hugo-blog-data是我本地的hugo博客文件夹，/app是容器内的hugo博客文件夹，这样就可以将本地的hugo博客文件夹挂载到容器内，这样就可以在本地编辑博客，然后在容器内运行hugo server来预览博客了。

## 进入容器

```shell
docker ps
docker exex -it <container id> /bin/bash
```

## 运行hugo server


现在已经进入了容器内部。

```bash
cd /app
hugo new site myblog
cd myblog
hugo new posts/my-first-post.md
```

这里我们创建了一个名为myblog的hugo博客，然后创建了第一篇博客。

下载[主题模板](https://themes.gohugo.io/)到myblog/themes/目录下，然后修改config.toml文件，将主题改为我们下载的主题。

```bash
hugo server --theme=/app/myblog/themes/hugo-theme-learn --bind=
```

hugo server 命令执行后， hugo server 将运行起来,并且会监听1313端口，我们可以通过浏览器访问本地址发布的博客网站了。

