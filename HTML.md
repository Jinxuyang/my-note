---
title: HTML学习笔记
date: 2020-03-28 20:01:31
tags:
---

[TOC]

# html

## 基础

### html文件的基本结构

```html
<!DOCTYPE HTML>
<!--注释是这样写的-->
<html>
    <head> 
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
        <title>这里是head,title里的内容会显示在浏览器标题栏里</title>
    </head>
    <body>
        <h1>这里是body</h1>
    </body>
</html>
```

## 标签

### 这些都很简单

#### body标签

一般网页需要展示出来的内容就存放在body标签里

<!--more-->

#### p标签

段落标签

#### hx标签

共有h1到h6六个从大到小的标签

#### em和strong标签

em是<em>斜体</em>，strong是<strong>加粗</strong>

#### span标签

span本身没有什么作用，它就是用来设置单独的样式用的

#### q标签

短文本引用，引用后会自动给引用的部分加上引号

#### blockquote标签

<blockquote>长文本引用，这个不会加引号，但是会产生缩进</blockquote>

![blockquote](http://img.mukewang.com/528c50ea000146a205520264.jpg)

#### br标签和\&nbsp;

html中回车空格是无效的，换行需要\<br />或\<br>(常用\<br />)

br与其他标签不同的是，br不需要成对出现

\&nbsp;就代表空格

#### hr标签

效果

<hr>

写成\<hr>或\<hr />

#### address标签

效果

<address>陕西西安</address>

#### code标签

效果

<code>while(1)</code>

#### pre标签

插入大段代码

<pre>
    #include<iostream>
    using namespace std;
    int main(){
		cout << "Hello Word" << endl;
	}
</pre>

### 列表/表格

#### ul ol标签

```html
<ul>
    <li>123</li>
    <li>456</li>
    <li>789</li>
</ul>
```

<ul>
    <li>123</li>
    <li>456</li>
    <li>789</li>
</ul>

无序列表\<ul>

```html
<ol>
    <li>123</li>
    <li>456</li>
    <li>789</li>
</ol>
```

<ol>
    <li>123</li>
    <li>456</li>
    <li>789</li>
</ol>

有序列表\<ol>

#### div标签

可以用来划分区域

#### table标签和caption标签

```html
<table summary=”这里的文字不会显示，增加表格可读性“>
    <tbody>
        <!-- 如果不加<thead><tbody><tfooter> , table表格加载完后才显示。加上这些表格结构， tbody包含行的内容下载完优先显示，不必等待表格结束后在显示，同时如果表格很长，用tbody分段，可以一部分一部分地显示。（通俗理解table 可以按结构一块块的显示，不在等整个表格加载完后显示。-->
        <caotion>这是标题</caotion>   
        <tr><!--表格的行-->
          <th>班级</th><!--th是表格的表头-->
          <th>学生数</th>
          <th>平均成绩</th>
        </tr>
        <tr>
          <td>一班</td><!--一个单元格，有几个就说明表格有几列-->
          <td>30</td>
          <td>89</td>
        </tr>
        <tr>
          <td>二班</td>
          <td>35</td>
          <td>85</td>
        </tr>
    </tbody>
</table>
```

<table summary=”这里的文字不会显示，增加表格可读性“>
    <tbody>
         <caotion>这是标题</caotion>
        <tr>
          <th>班级</th>
          <th>学生数</th>
          <th>平均成绩</th>
        </tr>
        <tr>
          <td>一班</td>
          <td>30</td>
          <td>89</td>
        </tr>
        <tr>
          <td>二班</td>
          <td>35</td>
          <td>85</td>
        </tr>
    </tbody>
</table>

### 进阶

#### a标签

```html
<a  href="目标网址"  title="鼠标滑过显示的文本">链接显示的文本</a>
```

<a  href="baidu.com"  title="鼠标滑过显示的文本">链接显示的文本</a>

默认情况下链接的网页是在档期浏览器窗口打开

```html
<a  href="目标网址"  title="鼠标滑过显示的文本" target="_blank">链接显示的文本</a>
```

z这样写就是在新窗口打开

#### malito标签

```html
<a href="mailto:981340404@qq.com?cc=jinxuyang3@gmail.com&subject=我觉得你很帅&body=真的">发送</a>
```

<a href="mailto:981340404@qq.com?cc=jinxuyang3@gmail.com&subject=我觉得你很帅&body=真的">发送</a>

mailto后若有多个参数的话，第一个参数用?开头，后面跟的参数用&开头

mailto后是**":"**,其他参数后是**"="**

![mailto](http://img.mukewang.com/52da4f2a000150b714280550.jpg)

#### img标签

```html
<img src="图片地址" alt="下载失败时的替换文本" title = "提示文本">
```

图片使用**src**标识位置

**alt**当图像不可见时，显示的文本

<img src="" alt="就显示这个">

**title**鼠标滑过时显示的文本

img也不需要成对出现

### 表单

**可以把用户输入内容传到服务端**

\<form>标签成对出现

```html
<form    method="post"   action="save.php">
        <label for="username">用户名:</label>
        <input type="text" name="username" />
        <label for="pass">密码:</label>
        <input type="password" name="pass" />
</form>
```

**method**数据的传输方式（get/post）

**action**数据被传送到的地方

**所有表单控件（文本框、文本域、按钮、单选框、复选框等）都必须放在  标签之间**

#### 文本/密码输入框

\<input />单独出现

```html
<form>
   <input type="text/password" name="名称" value="文本" />
</form>
```

type="text"时文本输入框

type="password"时是密码输入框

name：文本框命名

value：文本框默认值，不输入时显示的内容

<form>
    <input type="text" name="account" value="value里显示的" /><br />
    <input type="password" name="account" value="123123123" />
</form>

#### 多行文本输入

```html
<textarea rows="行数" cols="列数"></textarea>
```

<textarea rows="5" cols="5"></textarea>

#### 单选框/复选框

```html
<input type="radio/checkbox" value="值" name="名称" checked="checked" />
```

- **type**

=radio为单选框

=checkbox为复选框

- value提交到服务器的值
- name为控件命名
- check=checked时该项默认勾选

**同一组的单选按钮name一定要相同这样才能实现单选**

<form>
    <label>你是人</label>
    <input type="radio" value="1" name="单选" />
    <br \>
    <label>你不是🐖</label>
    <input type="radio" value="1" name="单选" />
</form>
<hr \>
<form>
	1<input type="checkbox" value="1" name="1" />
    2<input type="checkbox" value="1" name="2" />
    3<input type="checkbox" value="1" name="3" checked="checked"/>
</form>



#### 下拉列表框

```html
<select>
	<option value='1'>1</option>
    <option value='2' selected="selected">2</option>
</select>
```

- value是向服务器提交的值
- selected="selected"表示该项默认选中,没有这个默认第一项

<select>
	<option value='1'>1</option>
    <option value='2' selected="selected">2</option>
</select>

##### 使用下拉列表框进行多选

再\<select>中添加multiple="mutiple"

```html
<select multiple="multiple">
	<option value='1'>1</option>
    <option value='2'>2</option>
</select>
```

<select multiple="multiple">
	<option value='1'>1</option>
    <option value='2'>2</option>
</select>

#### 提交/重置按钮

```html
<input type="submit/reset" value="提交" />
```

- type="submit"这样设置才是提交按钮
- value按钮上显示的内容

<form>
    <input type="submit" value="提交" />
    <input type="reset" value="重置" />
</form>

#### label标签

本来要选中控件就需要点中控件本身，有了label就可以把某些文字与控件关联，实现点击文字就聚焦到控件上

```html
<form>
    <label for="1">你是🐖</label>
    <input type="radio" id="1" name="r"/>
    <label for="2">你是人</label>
    <input type="radio" id="2" name="r"/>
</form>
<form>
    <label for="2">你是🐖</label>
    <input type="radio" id="1" name="r"/>
    <label for="1">你是人</label>
    <input type="radio" id="2" name="r"/>
</form>
```

<form>
    <label for="1">你是🐖</label>
    <input type="radio" id="1" name="r"/>
    <label for="2">你是人</label>
    <input type="radio" id="2" name="r"/>
</form>

<form>
    <label for="2">你是🐖</label>
    <input type="radio" id="1" name="r"/>
    <label for="1">你是人</label>
    <input type="radio" id="2" name="r"/>
</form>