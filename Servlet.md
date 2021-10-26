---
title: Servlet学习笔记
date: 2020-05-30 00:04:09
tags:
	- java
categories: 
	- java
---

[TOC]

## 什么是Servlet

Servlet(Service applet)运行在服务器的小程序

Servlet就是一个**接口**，定义了某个Java类能被tomcat识别的规则

<!--more-->

### 如何使用

1. 新建一个类
2. 实现servlet的接口
3. 实现接口中的抽象类
4. 配置servlet

## Servlet的执行流程

![](https://i.loli.net/2020/05/30/zigWGXkjAqUTQS6.png)

## Servlet基础

### 配置Servlet

```xml
<servlet>
	<servlet-name>name</servlet-name>
    <servlet-class>Servlet的地址</servlet-class>
</servlet>

<servlet-mapping>
	<servlet-name>name</servlet-name>
    <url-pattern>访问Servlet的地址</url-pattern>
</servlet-mapping>
```

当

### Servlet启动时自动装载

```xml
<servlet>
//加上这些代码
    <loadon-startup>num</loadon-startup>
</servlet>
```

num越小优先级越高

### 使用注解

`@WebServlet("url")`

就不用在web.xml里面配置了



### 声明周期

第一次访问后，servlet创建，第二次访问时不需要创建，直接使用，当服务器关闭时，servlet销毁

## Servlet结构

Servlet接口下有两个实现类

HttpServlet和GenericServlet

### GenericServlet

除了service类外，对Servlet接口的其他类都进行了空实现

以后使用Servlet时，对该类进行继承就行

### HttpServlet

对http协议进行了封装，简化操作 

## Request对象

### 继承体系

```
ServletRequest --接口
继承
HttpServletRequest --接口
实现
org.apache.catalina.connector.RequestFacade(tomcat实现)

```

### 获取数据库

#### 获取请求行

1. getMethod获取请求方法
2. **getContextPath取虚拟目录**
3. getServletPath获取Servlet路径
4. getQueryString获取请求参数
5. **获取请求URI**
   1. getRequestURI
   2. getRequestURL
6. getProtocol获取协议版本
7. getRemoteAddr获取客户机IP地址

#### 获取请求头

1. getHeader(String name)通过请求头的名称获得请求头
2. Enumeration\<String> getHeaderNames()获取所有请求头的名称

#### 获取请求体

* 只有POST有请求体

1. 获取流对象
   - BufferReader getReader()获取字符输入流，只能操作字符数据
   - ServletInputStream getInputStream()获取字节输入流，可以操作所有数据类型
2. 从流对象中获取数据

#### 其他（重要）

1. 获取请求参数的通用方式（兼容GET&POST）
   1. String getParameter(String name)根据参数名获取参数值
   2. String[] getParameterValues(String name)根据参数名获取参数值的数组 -- hobby=吃饭&hobby=睡觉
   3. Enumeration\<String> getParameterNames()获取所有请求参数的名称
   4. Map<String,String[]> getParameterMap()获取所有参数的map集合

2. 请求转发

   1. 步骤

      1. 通过request对象获取请求转发器对象：RequestDispatcher get RequestDispatcher(String path)

      2. 使用RequestDispatcher对象进行转发：forward(ServletRequest request,ServletResponse response)

         `request.getRequestDispatcher("/path").forward(request,response)`

   2. 特点

      1. 浏览器地址栏不变化
      2. 只能转发到当前服务器内部资源
      3. 访问带转发是一次请求

3. 共享数据

   - 域对象：一个有作用范围的对象，可以在范围内共享数据

   - request域：代表一次请求的范围，一般用于请求转发的多个资源中共享数据
   - 方法：
     1. void setAttribute(String name,Object obj)存储数据
     2. Object getAttribute(String name):通过键获取值
     3. void removeAttribute(String name):通过键移除键值对

4. 获取ServletContext

   1. 