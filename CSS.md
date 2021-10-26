---
title: CSS学习笔记
date: 2020-04-26 11:57:42
tags:
---

[TOC]

# CSS

穿衣服

## CSS语法

![CSS](http://img.mukewang.com/52fde5c30001b0fe03030117.jpg)

选择符又名选择器，指明网页中要应用样式规则的元素

<!--more-->

**最后一条声明可以没有分号**

/*这样写注释\*/

## CSS的三种用法及优先级

### 内联式

直接写在标签中

```html
<p style="color:red">
    红
</p>
```

<p style="color:red">
    红
</p>
多条语句用;分开

```html
<p style="color:red;font-size:12px">红</p>
```

<p style="color:red;font-size:36px">红</p>

### 嵌入式

```html
<style type="text/css">
    p{
        color:blue;
    }
</style>
<p >
    我是蓝的
</p>
```

### 外部式

创建一个单独的css文件

```html
<link href="base.css" rel="stylesheet" type="text/css" />
```

使用上面这句引用，通常写在head标签中

### 优先级

**内联式 > 嵌入式 > 外部式**

## 选择器

### 标签选择器

```css
p{font-size:12px;}
```

### 类选择器

使用"**.**"开头

```html
.1{
	color:pink;
}
<span class="1">
    粉🐖
</span>
```

### ID选择器

```html
#1{
	color:pink;
}
<span id="1">
    粉🐖
</span>
```

### 类选择器和ID选择器的区别

1. ID选择器只可以使用一次，而类选择器可以使用多次

```html
<span id="1">我不是</span>
<span id="1">🐖</span>
```

这是错的嗷

2. 还可以这样写

```html
.1{
	color:pink;
}
.2{
	font-size:200px;
}
<span class=”1 2“>大粉🐖</span>
```

### 亲戚关系选择器hhh

#### 子选择器

```html
<p class="1">
    <span>🐖</span>
</p>
```



```css
.1>sapn{color:pink}
```

选择class名为1下的子元素\<span>

#### 后代选择器

```css
.1 span{color:pink}
```

与子选择器的区别：子选择器只选择他的直接后代，而后代选择器把他的儿子孙子重孙都选上了,都变成粉色了

### 通用选择器

```css
*{color:pink;}
```

这下所以标签的元素都成粉的了

### 伪类选择器

给不存在的标签（或标签的某种状态）设置样式

```css
a:hover{color:pink;}
```

当鼠标经过时变粉

### 分组选择器

```css
h1,span{color:pink;}
```

相当于

```css
h1{color:pink;}
span{color:pink;}
```

## 继承，优先级和重要性

### 样式的继承性

什么是继承性？

允许样式不仅应用于某个特定html标签，而且应用于后代

```html
p{color:red;}
<p>hhhhh<sapn>wllll</span></p>
```

hhhhh是红色，wllll也是红色

**但是某些CSS样式不具有继承性**

### 选择器的优先级

1、如果一个元素使用了多个选择器,则会按照选择器的优先级来给定样式。

2、选择器的优先级依次是: 内联样式 > id选择器 > 类选择器 > 标签选择器 > 通配符选择器

### 权值

**标签的权值为1，类选择符的权值为10，ID选择符的权值最高为100**

若多个选择器匹配到了一个标签上，那么谁的权值高就显示谁的样式

```css
p{color:red;} /*权值为1*/
p span{color:green;} /*权值为1+1=2*/
.warning{color:white;} /*权值为10*/
p span.warning{color:purple;} /*权值为1+1+10=12*/
#footer .note p{color:yellow;} /*权值为100+10+1=111*/
```

### !important

```css
p{color:red!important;}
p{color:green;}
<p class="first">三年级时，我还是一个<span>胆小如鼠</span>的小女孩。</p>
```

带有!important的会被应用，无论其他的权值有多高

## 设置样式

### 关于字体

#### font-family

```css
body{font-family:"Microsoft Yahei";}
body{font-family:"微软雅黑";}
```

#### font-size

```css
body{font-size:12px;}
```

#### font-weight字体粗细

```
p span{font-weight:bold;}
```

| 值                                  | 描述                                                        |
| :---------------------------------- | :---------------------------------------------------------- |
| normal                              | 默认值。定义标准的字符。                                    |
| bold                                | 定义粗体字符。                                              |
| bolder                              | 定义更粗的字符。                                            |
| lighter                             | 定义更细的字符。                                            |
| 100/200/300/400/500/600/700/800/900 | 定义由粗到细的字符。400 等同于 normal，而 700 等同于 bold。 |
| inherit                             | 规定应该从父元素继承字体的粗细。                            |

#### font-style字体样式

```
p{font-style:normal;}
```

| 值      | 描述                                   |
| :------ | :------------------------------------- |
| normal  | 默认值。浏览器显示一个标准的字体样式。 |
| italic  | 浏览器会显示一个斜体的字体样式。       |
| oblique | 浏览器会显示一个倾斜的字体样式。       |
| inherit | 规定应该从父元素继承字体样式。         |

#### color设置字体颜色

- 英文命令颜色

  ```css
  p{color:red;}
  ```

- RGB 颜色

  ```
  p{color:rgb(133,45,200);}
  ```

  ```
  p{color:rgb(20%,33%,25%);}
  ```

- 十六进制颜色

  ```
  p{color:#00ffff;}
  ```

#### font家族的缩写

```css
body{
    font-style:italic;
    font-weight:bold; 
    font-size:12px; 
    line-height:1.5em; 
    font-family:"宋体",sans-serif;
}
```

这么多行的代码其实可以缩写为一句：

```css
body{
    font:italic  bold  12px/1.5em  "宋体",sans-serif;
}
```

1. **想要使用这种简写方式，至少要指定font-size和font-family，其他属性未指定则会使用默认值**
2. **在缩写时 font-size 与 line-height 中间要加入“/”斜扛**

### 关于文本

```css
h1 {text-decoration:none}
```

#### text-decoration给文本添加修饰

| 值           | 描述                                            |
| :----------- | :---------------------------------------------- |
| none         | 默认。定义标准的文本。                          |
| underline    | 定义文本下的一条线。                            |
| overline     | 定义文本上的一条线。                            |
| line-through | 定义穿过文本下的一条线。                        |
| blink        | 定义闪烁的文本。                                |
| inherit      | 规定应该从父元素继承 text-decoration 属性的值。 |

#### text-indent为文本添加首行缩进

```
p{text-indent:2em;}
```

1em 等于当前的字体尺寸。

2em 等于当前字体尺寸的两倍。

例如，如果某元素以 12pt 显示，那么 2em 是24pt。

在 CSS 中，em 是非常有用的单位，因为它可以自动适应用户所使用的字体

#### line-height为文字间设置行间距/行高

```
p{line-height:1.5em;}
```

#### letter/word-spacing增加或减少字符间的空白

`letter-spacing`为文字或字母中间设置间隔

```css
h1{letter-spacing:50px;}
```

`word-spacing`为单词之间设置间距

#### text-align设置文本对齐方式

为块状元素设置对齐方式

```
h1 {text-align:center}
```

| 值      | 描述                                       |
| :------ | :----------------------------------------- |
| left    | 把文本排列到左边。默认值：由浏览器决定。   |
| right   | 把文本排列到右边。                         |
| center  | 把文本排列到中间。                         |
| justify | 实现两端对齐文本效果。                     |
| inherit | 规定应该从父元素继承 text-align 属性的值。 |

## 盒模型

### 概念

在CSS中，HTML中的标签，被分为三类

- 块状元素

  - ```html
    <div>、<p>、<h1>...<h6>、<ol>、<ul>、<dl>、<table>、<address>、<blockquote> 、<form>
    ```

- 内联元素

  - ```html
    <a>、<span>、<br>、<i>、<em>、<strong>、<label>、<q>、<var>、<cite>、<code>
    ```

- 内联块元素

  - ```html
    <img>、<input>
    ```

#### 块级元素

- 每个块级元素都从新的一行开始
- 元素高度、宽度、行高、顶边底边距都可设置
- 元素宽度在不设置的情况下，是他本身父容器的100%，即和父元素宽度一致

通过

```css
a{display:block;}
```

可以将内联元素转换为块状元素

#### 内联元素

- 和其他元素都在一行上
- 元素的高度、宽度及顶部和底部边距**不可**设置
- 元素的宽度就是它包含的文字或图片的宽度，不可改变

```css
div{display:inline;}
```

可以将块状元素转换为内联元素

#### 内联块状元素

- 和其他元素在一行上
- 高宽，行高，顶底边距都可设置

```css
div{display:inline-block;}
```

当display设置为none时，元素隐藏

### 使用

#### 盒模型基本知识

![](http://img.mukewang.com/543b4cae0001b34304300350.jpg)

- padding 是盒子里的内容到盒子边框的距离
- border 是盒子的边框
- margin 是盒子边框距离别的盒子边框的距离

![](http://img.mukewang.com/539fbb3a0001304305570259.jpg)

**css内定义的宽（width）和高（height），指的是填充以里的内容范围。**

**因此一个元素实际宽度（盒子的宽度）=左边界+左边框+左填充+内容宽度+右填充+右边框+右边界。**

#### background-color背景色

```
div{background-color:red;}//为块状元素设置
a{background-color:green;}//为行内元素设置
```

#### border为盒子添加边框

| 值             | 描述                                                         |
| :------------- | :----------------------------------------------------------- |
| *border-width* | 规定边框的宽度。参阅：[border-width](https://www.w3school.com.cn/cssref/pr_border-width.asp) 中可能的值。 |
| *border-style* | 规定边框的样式。参阅：[border-style](https://www.w3school.com.cn/cssref/pr_border-style.asp) 中可能的值。 |
| *border-color* | 规定边框的颜色。参阅：[border-color](https://www.w3school.com.cn/cssref/pr_border-color.asp) 中可能的值。 |

```
div{
    border-width:2px;
    border-style:solid;
    border-color:red;
}
```

可以简写为

```
div{
    border:2px  solid  red;
}
```



##### border-方向 为盒子的某一边框设置样式

```css
div{border-bottom/top/left/right:xxx xxx}
```

##### border-radius 为边框四个角分别设置圆角

```css
 div{border-radius: 20px 10px 15px 30px;}
```

顺序为左上、右上、右下、左下

也可以分开写：

```css
div{
    border-top-left-radius: 20px;
   border-top-right-radius: 10px;
   border-bottom-right-radius: 15px;
   border-bottom-left-radius: 30px;
}
```

如果四个圆角都为10px;可以这么写：

```css
div{ border-radius:10px;}
```

如果左上角和右下角圆角效果一样为10px，右上角和左下角圆角一样为20px，可以这么写：

```css
div{ border-radius:10px 20px;}
```

需要特别注意的：一个正方形，当设置圆角效果值为元素宽度一半时，显示效果为圆形。例如：

```css
 div {
        width: 200px;
        height: 200px;
        border: 5px solid red;
        border-radius: 100px/50%;
    }
```

#### padding为盒子设置填充

```
div{padding:20px 10px 15px 30px;}
```

上右下左

![](https://img.mukewang.com/5e95733a0001dead04210227.jpg)

其他设置方法与border-radius类似

#### margin为盒子设置外边距

```
div{margin:20px 10px 15px 30px;}
```

![](https://img4.mukewang.com/5e95747a0001a39505090231.jpg)

一样

## 布局模型

### 流动模型（Flow）

默认的网页布局模式

特征

- 块状元素会在所处的包含元素内，自上而下垂直延申分布(默认状态下块元素的宽度为100%)
- 内联元素都会在包含元素内从左到右水平分布

### 浮动模型(Float)

可以设置两个块状元素显示一行

```css
div{float:left/right;}
```

设置靠左or靠右

也可以分别设置，使之一个靠左一个靠右

```css
div1{float:left;}
#div2{float:right;}
```

### 层模型

**关于文档流可以看这篇https://blog.csdn.net/wayne1998/article/details/80230608**

层模型有三种形式：

1、**绝对定位**(position: absolute)

这条语句的作用将元素从**文档流**中拖出来，然后使用left、right、top、bottom属性相对于其最接近的一个具有定位属性的父包含块进行绝对定位。如果不存在这样的包含块，则相对于body元素，即相对于**浏览器窗口**。

2、**相对定位**(position: relative)

它通过left、right、top、bottom属性确定元素在**正常文档流中**的偏移位置。相对定位完成的过程是首先按static(float)方式生成一个元素(并且元素像层一样浮动了起来)，然后相对于**以前的位置移动，**移动的方向和幅度由left、right、top、bottom属性确定，偏移前的位置保留不动。

**我的理解是，先正常生成元素，然后再浮起来，相对原来的位置进行移动**

![](http://img.mukewang.com/541a4bfc0001abef05940489.jpg)

3、**固定定位**(position: fixed)

fixed：表示固定定位，与absolute定位类型类似，但它的相对移动的坐标是视图（**屏幕内的网页窗口**）本身。由于视图本身是固定的，它不会随浏览器窗口的滚动条滚动而变化，除非你在屏幕中移动浏览器窗口的屏幕位置，或改变浏览器窗口的显示大小，因此固定定位的元素会始终位于浏览器窗口内视图的某个位置，不会受文档流动影响

```css
#div1{
    width:200px;
    height:200px;
    border:2px red solid;
    position:fixed;
    left:100px;
    top:50px;
}
```

## 弹性盒模型

**这个好这个好https://www.jianshu.com/p/5856c4ae91f2**

弹性盒子由弹性容器和弹性子元素组成

```css
display:flex | inline-flex;
```

弹性子元素通常在弹性盒子内一行显示。默认情况每个容器只有一行。

### 常用属性

| 属性            | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| flex-direction  | 指定弹性容器中子元素排列方式                                 |
| flex-wrap       | 设置弹性盒子的子元素超出父容器时是否换行                     |
| flex-flow       | flex-direction 和 flex-wrap 的简写                           |
| align-items     | 设置弹性盒子元素在侧轴（纵轴）方向上的对齐方式               |
| align-content   | 修改 flex-wrap 属性的行为，类似 align-items, 但不是设置子元素对齐，而是设置行对齐 |
| justify-content | 设置弹性盒子元素在主轴（横轴）方向上的对齐方式               |

#### flex-direction

决定子元素排列方向

```css
.flex-container { flex-direction: row | row-reverse | column | column-reverse; }
```

| 值             | 描述                                     |
| -------------- | ---------------------------------------- |
| row            | 默认值。元素将水平显示，正如一个行一样。 |
| row-reverse    | 与 row 相同，但是以相反的顺序。          |
| column         | 元素将垂直显示，正如一个列一样。         |
| column-reverse | 与 column 相同，但是以相反的顺序。       |

![](https://upload-images.jianshu.io/upload_images/2326131-bbd36877856086ff.png?imageMogr2/auto-orient/strip|imageView2/2/w/796/format/webp)

#### flex-wrap

flex-wrap 属性规定flex容器是单行或者多行，同时横轴的方向决定了新行堆叠的方向。

| 值           | 描述                                               |
| ------------ | -------------------------------------------------- |
| nowrap       | 默认值。规定元素不拆行或不拆列。                   |
| wrap         | 规定元素在必要的时候拆行或拆列。                   |
| wrap-reverse | 规定元素在必要的时候拆行或拆列，但是以相反的顺序。 |

可以取三个值：
（1） nowrap (默认)：不换行。



![img](https://upload-images.jianshu.io/upload_images/2326131-b71b6e4c79ceb64b.png?imageMogr2/auto-orient/strip|imageView2/2/w/700/format/webp)

（2）wrap：换行，第一行在上方。



![img](https://upload-images.jianshu.io/upload_images/2326131-6de957f9ef4d43fa.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/700/format/webp)

（3）wrap-reverse：换行，第一行在下方。



![img](https://upload-images.jianshu.io/upload_images/2326131-b432b2461d51d73a.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/700/format/webp)

#### flex-flow

flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。

```css
.flex-container { flex-flow: <flex-direction> <flex-wrap> }
```

#### align-items

align-items 属性定义flex子项在flex容器的当前行的侧轴（纵轴）方向上的对齐方式。

| 值         | 描述                           |
| ---------- | ------------------------------ |
| stretch    | 默认值。项目被拉伸以适应容器。 |
| center     | 项目位于容器的中心。           |
| flex-start | 项目位于容器的开头。           |
| flex-end   | 项目位于容器的结尾。           |
| baseline   | 项目位于容器的基线上。         |

![img](https:////upload-images.jianshu.io/upload_images/2326131-b3099f9b3e8bea50.png?imageMogr2/auto-orient/strip|imageView2/2/w/617/format/webp)

#### justify-content



justify-content 用于设置或检索弹性盒子元素在主轴（横轴）方向上的对齐方式。

| 值            | 描述                                             |
| ------------- | ------------------------------------------------ |
| flex-start    | 默认值。项目位于容器的开头。                     |
| flex-end      | 项目位于容器的结尾。                             |
| center        | 项目位于容器的中心。                             |
| space-between | 项目位于各行之间留有空白的容器内。               |
| space-around  | 项目位于各行之前、之间、之后都留有空白的容器内。 |

![img](https:////upload-images.jianshu.io/upload_images/2326131-86f8477572e8b976.png?imageMogr2/auto-orient/strip|imageView2/2/w/637/format/webp)

### 弹性子元素属性

| 属性        | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| order       | 设置弹性盒子的子元素排列顺序。                               |
| flex-grow   | 设置或检索弹性盒子元素的扩展比率。                           |
| flex-shrink | 指定了 flex 元素的收缩规则。flex 元素仅在默认宽度之和大于容器的时候才会发生收缩，其收缩的大小是依据 flex-shrink 的值。 |
| flex-basis  | 用于设置或检索弹性盒伸缩基准值。                             |
| flex        | 设置弹性盒子的子元素如何分配空间。                           |
| align-self  | 在弹性子元素上使用。覆盖容器的 align-items 属性。            |

#### order属性

```css
.flex-container .flex-item { order: <integer>; }
```

<integer>：用整数值来定义排列顺序，数值小的排在前面。可以为负值，默认为0。



![img](https:////upload-images.jianshu.io/upload_images/2326131-5466d2ec3968ca3b.png?imageMogr2/auto-orient/strip|imageView2/2/w/751/format/webp)

#### flex-grow属性



```css
.flex-container .flex-item { flex-grow: <integer>; }
```

<integer>：一个数字，规定项目将相对于其他灵活的项目进行扩展的量。默认值是 0。



![img](https:////upload-images.jianshu.io/upload_images/2326131-189a57eada2a12a8.png?imageMogr2/auto-orient/strip|imageView2/2/w/802/format/webp)

#### flex-shrink属性



```css
.flex-container .flex-item { flex-shrink: <integer>; }
```

<integer>：一个数字，规定项目将相对于其他灵活的项目进行收缩的量。默认值是 1。



![img](https:////upload-images.jianshu.io/upload_images/2326131-b55c4a8caa6d3a90.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/700/format/webp)

#### flex-basis属性



```css
.flex-container .flex-item { flex-basis: <integer> | auto; }
```

<integer>：一个长度单位或者一个百分比，规定元素的初始长度。
 auto：默认值。长度等于元素的长度。如果该项目未指定长度，则长度将根据内容决定。

#### flex属性

flex 属性用于设置或检索弹性盒模型对象的子元素如何分配空间。

flex 属性是 flex-grow、flex-shrink 和 flex-basis 属性的简写属性。



```css
.flex-container .flex-item { flex: flex-grow flex-shrink flex-basis | auto | initial | inherit; }
```

| 值          | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| flex-grow   | 一个数字，规定项目将相对于其他元素进行扩展的量。             |
| flex-shrink | 一个数字，规定项目将相对于其他元素进行收缩的量。             |
| flex-basis  | 项目的长度。合法值："auto"、"inherit" 或一个后跟 "%"、"px"、"em" 或任何其他长度单位的数字。 |
| auto        | 与 1 1 auto 相同。                                           |
| none        | 与 0 0 auto 相同。                                           |
| initial     | 设置该属性为它的默认值，即为 0 1 auto。                      |
| inherit     | 从父元素继承该属性。                                         |

#### align-self属性



```css
.flex-container .flex-item {
    align-self: auto | stretch | center | flex-start | flex-end | baseline | initial | inherit;
}
```

| 值         | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| auto       | 默认值。元素继承了它的父容器的 align-items 属性。如果没有父容器则为 "stretch"。 |
| stretch    | 元素被拉伸以适应容器。                                       |
| center     | 元素位于容器的中心。                                         |
| flex-start | 元素位于容器的开头。                                         |
| flex-end   | 元素位于容器的结尾。                                         |
| baseline   | 元素位于容器的基线上。                                       |
| initial    | 设置该属性为它的默认值。                                     |
| inherit    | 从父元素继承该属性。                                         |

![img](https:////upload-images.jianshu.io/upload_images/2326131-8dc02c66cf79f0e8.png?imageMogr2/auto-orient/strip|imageView2/2/w/743/format/webp)



**作者：弓三水**
**链接：https://www.jianshu.com/p/5856c4ae91f2**
**来源：简书**
**写的很全很好很厉害，直接复制了，方便以后查阅**

