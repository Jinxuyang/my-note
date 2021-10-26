---
title: Linxu命令
date: 2020-09-14 12:28:37
tags:
---

## 防火墙

<!--more-->

### 开发80端口

` firewall-cmd --zone=public --add-port=80/tcp --permanent`

### 查询端口号80 是否开启

`firewall-cmd --query-port=80/tcp`

### 重启防火墙

` firewall-cmd --reload`

### 查询有哪些端口是开启的

`firewall-cmd --list-port`



### 参数说明

`--zone #作用域
--add-port=80/tcp #添加端口，格式为：端口/通讯协议
--permanent #永久生效，没有此参数重启后失效`