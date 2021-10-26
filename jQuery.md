---
title: jQuery学习笔记
date: 2020-04-27 16:04:06
tags:
---

[TOC]



## 环境搭建

```html
<!DOCTYPE HTML>
<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
        <script type="text/javascript" src="https://www.imooc.com/static/lib/jquery/1.9.1/jquery.js"></script>
        <title>环境搭建</title>
    </head> 
    <body>
        <script type="text/javascript"> alert($)
        </script>
    </body>
</html>
```

想要使用jQuery只需要在\<head>标签中

<!--more-->

```html
<script type="text/javascript" src="https://www.imooc.com/static/lib/jquery/1.9.1/jquery.js"></script>
```

**注意：一定千万不要在引用jquery的script标签里再写js了，重新写一个script标签**

可以在这里https://jquery.com/download/下载使用jQuery的其他版本

网速慢的话

https://www.bootcdn.cn/jquery/

这样操作

## 选择器

### id选择器

```
$( "#id" )
```

id若多个元素分配了相同的id，将只匹配该id选择集合的第一个DOM元素，按理来说id需要唯一

### 类选择器

```javascript
$(".class")
```

假如你想改变某一类标签内容的样式可以直接使用`$(.class).css()`,不再像原生js一样需要循环

### 标签选择器

```javascript
$("tag")
```

### 全能选择器

```javascript
$("*")
```

获取文档中的所有元素

### 层级选择器

文档中的所有的节点之间都是有这样或者那样的关系。我们可以把节点之间的关系可以用传统的家族关系来描述，可以把文档树当作一个家谱，那么节点与节点直接就会存在父子，兄弟，祖孙的关系了。

![](http://img.mukewang.com/5590e98b0001f60d06130229.jpg)

### 基本筛选选择器

![](http://img.mukewang.com/57cd1df2000146de06020498.jpg)

假如你使用标签选择器选取了所有div标签，但是你只想要第一个就可以这样

```javascript
$("div:first")//选择第一个
$("div:eq(0)")//选择索引值为0的那个
$("div:lt(1)")//选择索引值小于1的
```

### 内容筛选选择器

![](http://img.mukewang.com/57cd20bf0001a97f05290214.jpg)

1. :contains与:has都有查找的意思，但是contains查找包含“**指定文本**”的元素，has查找包含“**指定元素**”的元素
2. 如果:contains匹配的文本包含在元素的子元素中，同样认为是符合条件的。
3. :parent与:empty是相反的，两者所涉及的子元素，包括文本节点

### 可见性筛选选择器

![](http://img.mukewang.com/5590f6de0001e2b204460106.jpg)

我们有几种方式可以隐藏一个元素：

1. CSS display的值是none。
2. type="hidden"的表单元素。
3. 宽度和高度都显式设置为0。
4. 一个祖先元素是隐藏的，该元素是不会在页面上显示
5. CSS visibility的值是hidden
6. CSS opacity的值是0

### 属性筛选选择器

![](http://img.mukewang.com/57d654200001c46507360560.jpg)

用法

```javascript
$("一个元素集合[属性 | 属性 运算符 "value"]")
```

### 子元素筛选选择器

![](https://img3.mukewang.com/5bac45120001301105960331.jpg)

1. :first只匹配一个单独的元素，但是:first-child选择器可以匹配多个：即为每个父级元素匹配第一个子元素。这相当于:nth-child(1)
2. :last 只匹配一个单独的元素， :last-child 选择器可以匹配多个元素：即，为每个父级元素匹配最后一个子元素
3. 如果子元素只有一个的话，:first-child与:last-child是同一个
4.  :only-child匹配某个元素是父元素中唯一的子元素，就是说当前子元素是父元素中唯一的元素，则匹配
5. jQuery实现:nth-child(n)是严格来自CSS规范，所以n值是“索引”，也就是说，从1开始计数，:nth-child(index)从1开始的，而eq(index)是从0开始的
6. nth-child(n) 与 :nth-last-child(n) 的区别前者是从前往后计算，后者从后往前计算

### 表单元素选择器

![img](http://img.mukewang.com/5592040d0001f8eb04940441.jpg)

#### 表单对象属性筛选选择器

![img](http://img.mukewang.com/55920c2f0001198b04940201.jpg)

### 特殊选择器this

> this，表示当前的上下文对象是一个html对象，可以调用html对象所拥有的属性和方法。
> $(this),代表的上下文对象是一个jquery的上下文对象，可以调用jQuery的方法和属性值

## 属性与样式

### attr()与.removeAttr()

用来获取和设置元素属性

**attr()有4个表达式**

1. attr(传入属性名)：获取属性的值
2. attr(属性名, 属性值)：设置属性的值
3. attr(属性名,函数值)：设置属性的函数值
4. attr(attributes)：给指定元素设置多个属性值，即：{属性名一: “属性值一” , 属性名二: “属性值二” , … … }

**removeAttr()删除方法**

.removeAttr( attributeName ) : 为匹配的元素集合中的每个元素中移除一个属性（attribute）

```javascript
<a id="10086">移动</a>

$("#10086").attr(id,10001);
$("#10001").attr(id);
```

### .html()及.text()

.html() 获取集合中第一个匹配元素的HTML内容 或 设置每一个匹配元素的html内容，具体有3种用法：

1. .html() 不传入值，就是获取集合中第一个匹配元素的HTML内容
2. .html( htmlString ) 设置每一个匹配元素的html内容
3. .html( function(index, oldhtml) ) 用来返回设置HTML内容的一个函数

.text()

得到匹配元素集合中每个元素的文本内容结合，包括他们的后代，或设置匹配元素集合中每个元素的文本内容为指定的文本内容。，具体有3种用法：

1. .text() 得到匹配元素集合中每个元素的合并文本，包括他们的后代
2. .text( textString ) 用于设置匹配元素内容的文本
3. .text( function(index, text) ) 用来返回设置文本内容的一个函数





.html处理的是元素内容，.text处理的是文本内容

### .val()

**.val()方法**

主要是用于处理表单元素的值，比如 input, select 和 textarea。

1. .val()无参数，获取匹配的元素集合中第一个元素的当前值
2. .val( value )，设置匹配的元素集合中每个元素的值
3. .val( function ) ，一个用来返回设置值的函数

**.html(),.text(),.val()三种方法都是用来读取选定元素的内容；只不过.html()是用来读取元素的html内容（包括html标签），.text()用来读取元素的纯文本内容，包括其后代元素，.val()是用来读取表单元素的"value"值。其中.html()和.text()方法不能使用在表单元素上,而.val()只能使用在表单元素上；另外.html()方法使用在多个元素上时，只读取第一个元素；.val()方法和.html()相同，如果其应用在多个元素上时，只能读取第一个表单元素的"value"值，但是.text()和他们不一样，如果.text()应用在多个元素上时，将会读取所有选中元素的文本内容**。

### .addClass()和removeClass()

**addClass( className )方法**

1. .addClass( className ) : 为每个匹配元素所要增加的一个或多个样式名
2. .addClass( function(index, currentClass) ) : 这个函数返回一个或更多用空格隔开的要增加的样式名

**注意事项：**

```
.addClass()方法不会替换一个样式类名。它只是简单的添加一个样式类名到元素上
```

**.removeClass( )方法**

1. .removeClass( [className ] )：每个匹配元素移除的一个或多个用空格隔开的样式名
2. .removeClass( function(index, class) ) ： 一个函数，返回一个或多个将要被移除的样式名

**注意事项**

如果一个样式类名作为一个参数,只有这样式类会被从匹配的元素集合中删除 。 如果没有样式名作为参数，那么所有的样式类将被移除

### .toggleClass()

一次执行相当于addClass，再次执行相当于removeClass

**.toggleClass( )方法：**在匹配的元素集合中的每个元素上添加或删除一个或多个样式类,取决于这个样式类是否存在或值切换属性。即：如果存在（不存在）就删除（添加）一个类

1. .toggleClass( className )：在匹配的元素集合中的每个元素上用来切换的一个或多个（用空格隔开）样式类名
2. .toggleClass( className, switch )：一个布尔值，用于判断样式是否应该被添加或移除
3. .toggleClass( [switch ] )：一个用来判断样式类添加还是移除的 布尔值
4. .toggleClass( function(index, class, switch) [, switch ] )：用来返回在匹配的元素集合中的每个元素上用来切换的样式类名的一个函数。接收元素的索引位置和元素旧的样式类作为参数

**注意事项：**

1. toggleClass是一个互斥的逻辑，也就是通过判断对应的元素上是否存在指定的Class名，如果有就删除，如果没有就增加

### .css()

**获取：**

1. .css( propertyName ) ：获取匹配元素集合中的第一个元素的样式属性的计算值

```javascript
.css("background-color")
```



1. .css( propertyNames )：传递一组数组，返回一个对象结果

```javascript
.css(['width','height'])
```



**设置：**

1.  .css(propertyName, value )：设置CSS

   ```
   .css("background-color","red")
   ```

2. .css( propertyName, function )：可以传入一个回调函数，返回取到对应的值进行处理

   ```
   .css("width",function(index,value){
               //value带单位，先分解
               value = value.split('px');
               //返回一个新的值，在原有的值上，增加50px
               return (Number(value[0]) + 50) + value[1];
           })
   ```

3. .css( properties )：可以传一个对象，同时设置多个样式

```
css({
            'font-size'        :"15px",
            "background-color" :"#40E0D0",
            "border"           :"1px solid red"
        })
```

## 结点创建与属性

### 创建

```js
$("<div class='right'><div class='aaron'>动态创建DIV元素节点</div></div>")
```

### 插入

![img](http://img.mukewang.com/56cc12f800017b4104480146.jpg)

.append(), 内容在方法的后面，参数是将要插入的内容。

.appendTo()刚好相反，内容在方法前面，无论是一个选择器表达式 或创建作为标记上的标记它都将被插入到目标容器的末尾

```
append()前面是被插入的对象，后面是要在对象内插入的元素内容
appendTo()前面是要插入的元素内容，而后面是被插入的对象
```

**作用是一样的**