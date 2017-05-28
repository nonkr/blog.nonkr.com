---
title: CentOS7 学习笔记
date: 2017-05-28 12:33:58
categories:
tags: CentOS7
---

## systemctl
CentOS7下使用systemctl替代service来管理系统服务

### 常用命令

* 查看所有服务

```bash
systemctl list-unit-files
```

* 查看服务状态

```bash
systemctl status firewalld.service
```

<!-- more -->

* 启动服务

```bash
systemctl start firewalld.service
```

* 停止服务

```bash
systemctl stop firewalld.service
```

* 开启随系统启动

```bash
systemctl enable firewalld.service
```

* 关闭随系统启动

```bash
systemctl disable firewalld.service
```

## firewalld
Centos7中默认将原来的防火墙iptables升级为了firewalld，firewalld跟iptables比起来至少有两大好处：
1. firewalld可以动态修改单条规则，而不需要像iptables那样，在修改了规则后必须得全部刷新才可以生效；
2. firewalld在使用上要比iptables人性化很多，即使不明白“五张表五条链”而且对TCP/IP协议也不理解也可以实现大部分功能。

### 最简单的使用方式

* 放行某个端口

```bash
firewall-cmd --zone=public --add-port=80/tcp --permanent
```
上述命令写入规则到/etc/firewalld/zones/public.xml中

* 重启firewalld

```bash
firewall-cmd --reload
```

* 查询某个端口是否打开

```bash
firewall-cmd --query-port=80/tcp
```

# 参考文献
[5分钟理解Centos7防火墙firewalld](http://www.cnblogs.com/excelib/p/5150647.html)
