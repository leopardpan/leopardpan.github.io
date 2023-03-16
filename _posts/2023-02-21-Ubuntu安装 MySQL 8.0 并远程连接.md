---
layout: post

title: "Ubuntu安装 MySQL 8.0 并远程连接"

date: 2023-02-21

description: "主要介绍在Ubuntu系统下面安装MySQL 8.0 并使用Navicat或者Vscode远程连接的具体步骤和坑"

tag: Linux
---
libpython3.7m.so.1.0: /usr/local/lib/libpython3.7m.so.1.0

# 安装 MySQL

```bash
sudo apt update
sudo apt install mysql-server
```

# 查看默认用户名和密码

新版的MySQL安装后，默认用户名不是root，为了方便，一般我们需要修改成我们想要的用户名和密码。进入配置文件：

```bash
vim /etc/mysql/debian.cnf 
```

结果如下：

![debian.cnf结果](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230224080255.png)

默认的密码为：

```console
user     = debian-sys-maint
password = VDgQcUjzyfPfgxbA
```

# 登录数据库并修改用户名和密码

```bash
mysql -udebian-sys-maint -p
```

查看mysql中默认数据库：（记得sql结尾要加分号）

```sql
show databases;
```

![show databases results](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230224080909.png)

使用数据库mysql:

```sql
use mysql;
```

![use mysql results](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230224081258.png)

将root密码设置为空：

```sql
update user set authentication_string='' where user='root'; 
```

清空root用户默认的密码：

```mysql
select user,authentication_string from user where user='root';
```

![清空root用户默认的密码](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230224082514.png)

为root设置密码：ALTER user

```sql
ALTER user 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
```

查看设置root用户和密码：

```sql
select user,authentication_string from user where user='root';
```

![查看设置root用户和密码](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230224083500.png)

退出SQL:

```sql
quit;
```

在mysql8.0之后的版本中，password函数已被取消，加密方式不再使用mysql_native_password，换成了caching_sha2_password.

原本的sql语句为：

```sql
UPDATE user SET authentication_string=password("密码") WHERE user="root";
1
```

对应的写法可以改为：

```sql
UPDATE user SET authentication_string=SHA1("密码") WHERE user="root";
```

# 配置MySQL远程登陆

```shell
#修改配置文件，注释掉 bind-address = 127.0.0.1
$ sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

# 保存退出，然后进入mysql服务，执行授权命令：(输入密码123456)
$ mysql -uroot -p
```

![授权命令](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230224084410.png)

localhost即本地主机的域名，如同：www.baidu.com ，也是本机ip地址，整个127.* 网段通常被用作loopback网络接口的默认地址，按照惯例通常设置为127.0.0.1。我们当前这个主机上的这个地址，别人不能访问，即使访问，也是访问自己。因为每一台TCP/IP协议栈的设备基本上都有local/127.0.0.1.

进入数据库之后，查看当前主机配置信息为localhost，需要修改为%：(目的是为了让所有主机都能访问)

```sql
use mysql;
select host from user where user='root';
```

将Host设置为通配符%:

```sql
update user set host = '%' where user ='root';
```

修改后要刷新一下:

```sql
flush privileges;
```

查看是否修改成功:

```sql
select host from user where user='root';
```

![已经修改成功](_posts/image/2023-02-21-Ubuntu安装MySQL8.0并远程连接/1677199916004.png)

退出数据库后重启数据库服务：(然后就可以在数据库工具远程MySQL数据库了。)

```shell
sudo /etc/init.d/mysql restart #重启mysql服务
```

# 在远程数据库远程连接MySQL数据库了

首先是进行测试，无论您如何安装它，MySQL 都应该自动开始运行。要对此进行测试，请检查其状态。

```shell
sudo systemctl status mysql
```

![MySQL已经自动运行](_posts/image/2023-02-21-Ubuntu安装MySQL8.0并远程连接/1677200253300.png)

然后，使用 ss 命令检查开放端口：

```shell
sudo ss -ltn 
```

![开放端口如下](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230224085823.png)

可以看出，我们的服务器正在侦听端口 3306 和 33060 上的连接。这些是与 HTTP 和 MySQL 相关的端口。

在我们的本地系统上使用ss或者nmap localhost命令时，我们绕过了防火墙。实际上，这些命令显示处于侦听状态的端口，但这并不一定意味着端口对 Internet 开放，因为我们的防火墙可能拒绝连接。

使用以下命令检查 ufw 防火墙的状态。

```shell
sudo ufw status verbose
```

可以发现，防火墙状态为不活动。

![1677200490189](_posts/image/2023-02-21-Ubuntu安装MySQL8.0并远程连接/1677200490189.png)

如果防火墙状态为active状态， ufw 拒绝传入连接。由于端口 80 和 3306 尚未添加为例外，因此 HTTP 和 MySQL 无法接收传入连接，尽管ss并nmap报告它们处于侦听状态。

让我们使用以下命令为这些端口添加例外。

```shell
sudo ufw allow 80/tcp
sudo ufw allow 3306/tcp
```

[navicat安装破解教程](https://www.jianshu.com/p/9c4c499429da)

使用ifconfig查看服务器ip地址，然后在navicat中连接数据库。

![连接数据库](_posts/image/2023-02-21-Ubuntu安装MySQL8.0并远程连接/1677202388976.png)
连接结果如下：

![连接结果](_posts/image/2023-02-21-Ubuntu安装MySQL8.0并远程连接/1677202138195.png)

也可以用VSCode来管理MySql数据库：

1. 点击左侧扩展插件，然后搜索下MySql；
2. 安装过后，我们就需要连接了，直接快捷键，Ctrl+Shift+P，打开窗口；
3. 设置如下：
   1. ![设置如下](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230224093459.png)
   2. 端口号默认3306
   3. 连接结果如下：![连接结果如下](https://cdn.jsdelivr.net/gh/ChanJeunlam/PicgoBed/blogs/pictures/20230224093706.png)

# 参考链接

[Ubuntu安装MySQL8.0，并用Navicat远程连接_任求其-zzx的博客-CSDN博客](https://blog.csdn.net/qq_43685040/article/details/108732648)

[Ubuntu 20.04 安装 MySQL 8.0 并且远程连接数据库(包括后续遇到的新坑)_Nymph2333的博客-CSDN博客_executing /lib/systemd/systemd-sysv-install enable](https://blog.csdn.net/u014378628/article/details/118406005)

[在 Ubuntu 22.04 上安装 Python 3.9（多版本适用） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/506491209)

[Index of python-local/3.7.12 (huaweicloud.com)](https://mirrors.huaweicloud.com/python/3.7.12/)

[在Ubuntu 18.04上安装不同版本的python及选择默认Python_WMLCOLIN的博客-CSDN博客](https://blog.csdn.net/weixin_42919435/article/details/109523985)
