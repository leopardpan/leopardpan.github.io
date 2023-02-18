---
layout: post

title: "Linux下Flatpak的安装与使用（以安装OBS为例)"

date: 2023-02-18

description: "Linux下Flatpak的安装与使用（以安装OBS为例)"

tag: Linux
---
软件包管理是任何Linux发行版的重要功能之一，可简化Linux应用程序的安装和维护方法。不同的Linux发行版采用不同的方法来打包和分发软件。

但是对于某些切换到不同的Linux发行版来的人说，相同的功能有时反而会成为绊脚石。他们发现很难理解新的软件包管理器，并且无法安装应用程序。为了使用多个程序包管理器解决此类问题，Linux发行版已经发展出了通用的包管理系统，如Snap、Appimage和Flatpak。

# 什么是Flatpak？

Flatpak是一个通用的软件包管理系统，用于在任何Linux发行版上构建和分发应用程序。您无需学习特定于发行版的软件包管理器即可安装Flatpak应用。它为所有Linux发行版提供了一个命令行实用程序，以下载，安装和更新该应用程序。

# 什么是Flathub？

Flathub 是一个包含了几乎所有 flatpak 应用的仓库，可为Linux系统提供大量的应用程序和游戏。它还为想要构建，分发和定期更新应用程序的开发人员提供了构建服务。

# 设置Flatpak

```bash
# 安装flatpak
sudo apt install flatpak
# 安装图形化界面，但是我没有安装成功
sudo apt install gnome-software-plugin-flatpak
# 添加flathub仓库
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

# 从Flathub商店下载和安装Flatpak应用程序

## 安装和运行OBS

### 从Flathub商店下载

* 打开[flathub](https://flathub.org/home)网站，搜索OBS，点击安装
* 这时候，com.obsproject.Studio.flatpakref会被安装到/home/hugo/下载路径下
* 安装flatpakref后缀文件
  * ```bash
    flatpak install --from com.obsproject.Studio.flatpakref
    ```
* 安装完成后，可以查找到OBS
  * ```bash
    flatpak list # 查看OBS的app id
    ```
* 运行OBS
  * ```bash
    flatpak run <app id>
    ```

### 搜索安装

```bash
flatpak search obs
flatpak install <app id>
flatpak run <app id>
```

### 查看安装结果

```bash
flatpak list
```

![已经安装的应用](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230218164328.png)
