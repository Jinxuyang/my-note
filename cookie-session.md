---
title: cookie&session
date: 2020-06-01 00:48:08
tags:
---

## 会话

### 什么是会话

一次会话中包含多次请求和响应

一次会话：浏览器第一次给服务器发送请求，会话建立，指导一方断开为止

<!--more-->

### 有什么用

在一次会话范围内的多次请求共享数据

### 怎么实现 

1. 客户端会话技术（数据存放在客户端）：Cookie
2. 服务端会话技术（数据存放在服务端）：Session

## Cookie

### 如何使用

1. 创建Cookie对象】
   1. new Cookie(String name,String value)
2. 发送Ciookie对象
   1. response.addCookie(Cookie cookie)
3. 获取Cookie
   1. Cookie[] request.getCookies()



### 原理

- 基于请求头Cookie和响应头Set-Cookie实现

### 细节

1. 一次可以发送多个Cookie（创建多个Cookie对象，多次调用AddCookie）
2. Cookie可以保存多长时间
   1. 默认情况下，浏览器关闭后Cookie销毁
   2.  持久化存储
      1. setMaxAge(int seconds)
         1. 正数，将Cookie数据写道硬盘中，设置Cookie存货时间，单位s
         2. 负数，默认值，结束会话删除
         3. 0，删除Cookie信息

3. Cookie的获取范围
   1. 

### 特点和作用

1. cookie储存在客户端
2. 浏览器对于单个Cookie大小有限制4kb，对一个域名下总cookie数量有限制20个



1. 

## Session

### 使用

1. 获取HttpSession对象(域对象)
   1. request.getSession()

2. 方法
   1. Object getAttribute(String name)
   2. void setAttribute(String name,Object value)
   3. void removeAttribute(String name)

### 原理

Session的实现依赖于Cookie

首次使用时服务器向浏览器发送一个Cookie包含一个JSESSIONID，再次访问时，浏览器向服务器发送JSESSIONID，服务器通过这个ID，确保两次访问使用的是同一个HttpSeesion对象

### 细节

1. 客户端关闭服务器不关闭，两次访问不是同一个Session
   1. 若需要相同可以设置Cookie的存活时间
2. 服务器关闭客户端不关闭，两次访问不是同一个Session
   1. session钝化：
   2. session活化
3. Session失效时间
   1. 服务器关闭
   2. 调用一个自杀方法
   3. 默认失效时间30min



### 特点

1. 存储一次会话的数据，存在服务端
2. 可以存储任意类型数据，任意大小
3. 比Cookie安全