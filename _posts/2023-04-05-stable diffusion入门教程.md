---
layout: post
title: stable diffusion入门教程
date: 2023-04-05
description: "stable diffusion入门教程"
tag: 工具箱
---
## 1. 什么是stable diffusion？

## 2. 环境搭建

[python3.10.6](https://www.python.org/ftp/python/3.10.6/python-3.10.6-amd64.exe)

也可以从python release页面下载:
[https://www.python.org/downloads/windows/](https://www.python.org/downloads/windows/)

项目地址：
[stable diffusion](https://github.com/AUTOMATIC1111/stable-diffusion-webui)

拉取到本地：

```bash
https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
```

可能会遇到的问题：

windows下已经存在了其他的python版本，要先切换默认的python版本：

```bash
py --version # 查看当前默认的python版本
py -0p # 查看当前系统中安装的python版本
py -3.x -m ensurepip --default-pip # 切换默认的python版本
```

或者找到“系统属性”，点击“环境变量”，在“系统变量”中找到“Path”，在“Path”中应该已经有了两个python路径，删除其中一个python路径或者把其中想要的路径上移到最前面即可。

如果已经用了其他版本的python，要先去stable-diffusion-webui文件夹下面的venv删除所有文件再进行操作。

## 下载stable diffusion

运行webui-user.bat，第一次会下载所需要的whl和依赖包，可能会比较慢，耐心等待。

第一次运行出现的问题：
gfpgan安装失败

[解决方案](https://blog.csdn.net/weixin_40735291/article/details/129153398)

* 无法安装gfpgan的原因是**网络问题，就算已经科学上网，并设置为全局，也无法从github上下载源代码，从而导致install失败。**
* 解决方法是直接到github下载 [GFPGAN](https://github.com/TencentARC/GFPGAN) 代码到本地，并进行 **本地安装** 。
* 因为stable diffusion会在其根目录创建虚拟python环境venv，因此安装方法与github有所不同。可参考以下方法：
  * 从github将GFPGAN的源文件下载到本地，这一步可以使用git clone也可以直接下载zip文件。下载后，解压（如果用git clone就不需要）到 `d:\\stable-diffusion-webui\venv\Scripts`目录下（stable-diffusion-webui是你stable diffusion webui的根目录，这个地址只是我电脑中的，请根据自己放的位置调整）。
  * 打开cmd，cd到 `d:\\stable-diffusion-webui\venv\Scripts\GFPGAN-master`下。
  * 使用命令 `d:\\stable-diffusion-webui\venv\Scripts\python.exe -m pip install basicsr facexlib`安装GFPGAN的依赖。
  * 再使用 `d:\\stable-diffusion-webui\venv\Scripts\python.exe -m pip install -r requirements.txt`安装GFPGAN的依赖。
  * 使用 `d:\\stable-diffusion-webui\venv\Scripts\python.exe setup.py develop`安装GFPGAN。

如果文件路径有空格，要加上双引号。
这里的python.exe -m表示使用python.exe这个文件来执行模块pip，也就是pip.py这个文件。

安装open_clip失败

[解决方案](https://www.bilibili.com/read/cv22604427/)

还有其他一些小问题，很好解决的。

比如说从github上面拉取Stability-AI
/stablediffusion出现 ``Failed to connect to github.com port 443: Timed out``的问题，可以换国内的镜像源解决。

```shell
git clone https://hub.fgit.gq/Stability-AI/stablediffusion.git "D:/software1/stable diffusion/stable-diffusion-webui/repositories/stable-diffusion-stability-ai" 
```

## 运行stable diffusion

训练模型时报错：

``torch.cuda.OutOfMemoryError: CUDA out of memory. Tried to allocate 128.00 MiB (GPU 0; 4.00 GiB total capacity; 2.20 GiB already allocated; 44.64 MiB free; 2.41 GiB reserved in total by PyTorch) If reserved memory is >> allocated memory try setting max_split_size_mb to avoid fragmentation.  See documentation for Memory Management and PYTORCH_CUDA_ALLOC_CONF``

解决方法：

1. 降低batch_size
2. 换显卡
3. repositories\stable-diffusion-stability-ai\scripts\txt2img.py
   1. parser.add_argument的–n_samples的default改为1
   2. `model.to(device)`后添加 `.half()` 没找到这个在哪
4. 1

## Civitai

自己训练的照片不太满意，可以用Civitai的模型进行训练。

https://civitai.com/models/8109/ulzza

https://civitai.com/models/6424/chill

## 其他

txt2img只是stable diffusion最简单的功能，还可以img2img等等。
