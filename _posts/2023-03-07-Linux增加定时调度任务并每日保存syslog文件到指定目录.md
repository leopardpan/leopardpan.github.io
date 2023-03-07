---
layout: post
title: "Linux增加定时调度任务并每日保存syslog文件到指定目录"
date: 2023-03-07
description: "介绍Linux增加定时调度任务并且使用SHELL脚本编程，将日志文件syslog备份到指定目录"
tag: Linux
---
# 日志文件备份

有时候，为了系统稳定性,除了在/var/log通过回滚机制生成各种日志文件以排查系统故障之外，我们也想把这些关键的日志文件保存到指定文件夹，防止突然断电或者磁盘分区被破坏之后丢失关键的日志信息。

代码如下：（文件名为backup_sysconfig.sh）

```shell
#! /bin/bash
# this script is used to backup the syslog file in /var/log dir everyday
echo `date`
cp /var/log/syslog /home/hugo/test_BASH/log
# adding the user with x permission
chmod u+x syslog
# chnage the firname
today=$(date +%y%m%d)
echo $today
mv ./log/syslog ./log/syslog_$today # 同一个文件夹下面就相当于重命名
```

## 定时调度任务
crontab 的语句是“分 时 日 月 周 命令”.

开启定时调度任务，每天凌晨1点执行一次

```shell
crontab -e
# 0 1 * * * /home/hugo/test_BASH/back_sysconfig.sh
```
显示`crontab: installing new crontab`，表示定时任务已经开启。

查看定时任务

```shell
crontab -l
```

开启或者关闭定时任务

```shell
service crond start
service crond stop
```

发现报错：`Failed to start crond.service: Unit crond.service not found.`

删除调度任务：

* crontab -r ，删除所有任务调度工作
* 直接编辑 vim /etc/crontab ，文件内容如下：

```console
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
# You can also override PATH, but by default, newer versions inherit it from the environment
#PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
```
