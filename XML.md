---
title: XML学习笔记
date: 2020-05-31 21:50:17
tags:
	- xml
	- 学习笔记
categories: 
	- 学习笔记
---

## 什么是XML

XML(Extensible Markup Language)**可扩展**标记语言

可扩展：标签都是可以自定义的

语法严格

<!--more-->

## 啥用

1. 存储数据
   1. 配置文件
   2. 在网络中传输

## 基本语法

```xml
<? version = '1.0' ?>
<users>
	<user id = '1'>
        <name>1</name>
        <age>18</age>
    </user>
    
    <user id = '2'>
        <name>1</name>
        <age>18</age>
    </user>
</users>
```

1. 第一行必须为版本声明（必须是第一行，第一行是空行都不行）
2. 有且只能有一个根标签(\<users>\</users>)
3. 属性值必须用 引号引起来（单双都行）
4. 标签都是成对出现（除首行）
5. 标签名称区分大小写

