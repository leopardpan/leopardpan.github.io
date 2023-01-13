---
layout: post
title: "在Ubuntu上配置python环境"
date: 2023-01-13
description: "主要介绍了在Ubuntu下如何安装Pycharm,python,安装git和配置git环境等内容"
tag: Linux
---   

## 1.安装Pycharm
Pycharm现在也以snap软件包的形式提供，可以使用命令行的方式进行安装
```bash
sudo snap install pycharm-community --classic
```

## 2.安装python
```bash
sudo apt install python3
```
查看python版本
```bash
python3 --version
```
## 3.安装git
```bash
sudo apt install git
```
查看git版本
```bash
git --version
```
## 4.配置git环境
因为Gitee会用到SSH，因此需要在shell里检查是否可以连接到Gitee.
```bash
ssh -T git@gitee.com
```
如果出现以下内容，说明可以连接，需要进行下一步的配置。
```bash
Warning: Permanently added ‘***’ (RSA) to the list of known hosts.
Permission denied (publickey).
```
去注册一个Gitee账号，然后配置邮箱。

接着，安装SSH Keys(这一步一定要在~/.ssh目录下进行，否则会报错)。
```bash
cd ~/.ssh
ls
```
如果没有id_rsa和id_rsa.pub文件，就需要生成SSH Keys。
```bash
# 备份并移除已经存在的ssh keys,以防万一
mkdir backup
mv id_rsa* backup
rm id_rsa*
# 生成ssh keys
ssh-keygen -t rsa -C "***@***.com"
```
运行的时候会出来输入文件名，就输入id_rsa就可以

接着又会提示你输入两次密码（该密码是你push文件的时候要输入的密码，而不是github管理者的密码），

当然，你也可以不输入密码，直接按回车。那么push的时候就不需要输入密码，直接提交到gitee上了。

发现，id_rsa（私钥）和id_rsa.pub（公钥）这两个文件被创建了。

最后一步，将公钥添加到Gitee上。
（1）利用gedit/cat命令，查看id_rsa.pub的内容
（2）在GitHub中，依次点击Settings -> SSH Keys -> Add SSH Key，将id_rsa.pub文件中的字符串复制进去，注意字符串中没有换行和空格。
(3) 检查SSH Keys是否添加成功
```bash
ssh -T git@gitee.com
```
如果出现以下内容，说明可以连接，需要进行下一步的配置。
```bash
Hi Chamjeumlam! You’ve successfully authenticated, but GitHub does not provide shell access.
```

通过以上的设置之后，就能够通过SSH的方式，直接使用Git命令访问Gitee托管服务器了.

## 5.配置Pycharm的python
在Pycharm中，选择File -> Settings -> Project -> Project Interpreter -> Add -> Existing environment -> 选择python的安装路径 -> OK
python的安装路径可以通过命令行查看
```bash
which python3
```
## 6.配置Pycharm的git
在Pycharm中，选择File -> Settings -> Version Control -> Git -> Path to Git executable -> 选择git的安装路径 -> OK
git的安装路径可以通过命令行查看
```bash
which git
```
## 7.配置Pycharm的gitee
下载安装gitee
    1、在Setting中选择Plugins#
    2、在Marketplace下搜索框中搜索gitee
    3、点击Install进行下载安装
    4、安装完后点击Restart IDE
登陆码云
    1、File->Setting->搜索gitee#
    2、点击Add account#
    3、输入账号密码进行登陆
登陆成功后，会有登陆信息提示.
## 8. 建立远程仓库并提交代码
在Pycharm中，选择VCS -> Import into Version Control -> Share Project on Gitee -> 输入Gitee的用户名和密码 -> OK

做完这一步，提示错误：successfully created project on Gitee but initial commit failed.
需要进行设置，告诉gitee是谁在运行和更改代码。
Git 全局设置:
```bash
git config --global user.name "ChanJeunlam"
git config --global user.email "1120952003@qq.com"
```
创建 git 仓库:
```shell
mkdir InterAutoTest_W
cd InterAutoTest_W
git init 
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin https://gitee.com/chanjeunlam/InterAutoTest_W.git
git push -u origin "master"
```
已有仓库?
```
cd existing_git_repo
git remote add origin https://gitee.com/chanjeunlam/InterAutoTest_W.git
git push -u origin "master"
```

## 9. Pycharm中其他设置
更改字体大小：
1、点击顶部菜单栏中的File选项，接着点击Settings选项；2、依次打开Editor、Font选项；3、根据需要设置字体和字号即可。





