---
layout: post
title:  "brpc框架TCP连接数异常上涨问题排查"
categories: jekyll update
tag: trouble-shooting
---
# 背景
  11.64.167.116机器tcp连接数达到18w，且只增不减。
  进入个人github主页，创建一个Repository。
  ![tcp_connection_growth.png](/images/posts/2023-03-09-TCP_connections_trouble_shooting/tcp_connection_growth.png)
  
# 定位
  
## 初步分析
  tcp连接增长的节点，tcp重传率存在较大毛刺。因此初步断定，tcp连接数增长是由于网络异常导致
  ![TCP_retransmission.png](/images/posts/2023-03-09-TCP_connections_trouble_shooting/TCP_retransmission.png)

  对于同一个请求客户端ip，建立多个连接。以11.140.138.214为例，是ip所属的客户端采用brpc的CONNECTION_TYPE_SINGLE方式，理论上不会存在多个连接。
  ![multi_connections.png](/images/posts/2023-03-09-TCP_connections_trouble_shooting/multi_connections.png)

## 验证
  通过iptables -I INPUT -s 11.140.138.214 -ptcp --dport 44555 -j DROP模拟断网，用iptables -L -n --line-numbers，iptables -D INPUT 1将禁用释放出来。发现每次11.140.138.214都会重新建立一个连接
  ![verify_multi_connection.png](/images/posts/2023-03-09-TCP_connections_trouble_shooting/verify_multi_connection.png)

# 解决
## 升级
  设置ServerOptions.idle_timeout_sec
  并且将gflag: -log_idle_connection_close置为true

  详见：[关闭brpc闲置连接](https://github.com/apache/brpc/blob/master/docs/cn/server.md#%E5%85%B3%E9%97%AD%E9%97%B2%E7%BD%AE%E8%BF%9E%E6%8E%A5)。

## 后验
  断网后恢复网络，开始存在多个连接。过idle_timeout_sec时间后，闲置连接被释放，并打印log
  ![after_disconnected.png](/images/posts/2023-03-09-TCP_connections_trouble_shooting/after_disconnected.png)
  ![released_connection.png](/images/posts/2023-03-09-TCP_connections_trouble_shooting/released_connection.png)
  ![print_log.png](/images/posts/2023-03-09-TCP_connections_trouble_shooting/print_log.png)

# 补充
  理论上连接数可以达到几百万，目前这个量级的连接数不会对服务性能造成影响，参考[一台机器最多能撑多少个TCP连接](https://zhuanlan.zhihu.com/p/290651392?utm_oi=33526380494848)