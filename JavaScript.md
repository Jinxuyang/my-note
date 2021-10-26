---
title: JavaScript学习笔记
date: 2020-03-30 20:51:07
tags:
---

[TOC]

## 插入JS

使用\<script>\</script>标签

<q>\<script type="text/javascript"></q>固定写法

```html
<script type="text/javascript">
    JS写在这里
</script>
```

JS也可以单独存在类似CSS

在JS文件中不需要\<script>标签

<!--more-->

```html
<script src="js文件的路径"></script>
```

通过上面这句来引用js文件

JS一般放在\<head>或\<body>里，浏览器读到时就会运行

## 基础

1. js的每句代码结束都需要;
2. 用//添加注释，/\*注释内容\*/实现多行注释
3. 使用var声明变量
   - JS中变量区分大小写
4. if和else用法与C语言几乎一样
5. 函数使用function定义

```javascript
function name(){
    code;
}
```

定义好后不会自己执行，需要调用

### 输出

#### document.write()

1. 直接使用引号

```javascript
document.write("哈哈");
```

2. 输出变量

```javascript
var str="hahah";
document.write(str);
```

3. 输出多项内容

```javascript
var str="java";
documen.write(str+"script");
```

4. 输出html标签

```javascript
documen.write("<br>");
```

#### alert()

```javascript
alert(字符串或变量)；
```

弹出一个消息对话框，包含一个确定按钮

**点击确定前不能进行其他任何操作**

### 互动

#### confirm()

```javascript
confirm(str);
```

str是在对话框中显示的内容

返回值是Boolean

用户点击确定时返回ture，取消返回false

#### prompt()

```javascript
prompt(str1,str2);
```

str1是在对话框中显示的文本

str2是文本框中的内容

点确定返回文本框中的内容，取消返回null

### 窗口

#### window.open()

**注意使用单引号**

```javascript
window.open('URL','窗口名称','参数字符串,多个用逗号隔开');
```

- URL可选，在窗口中显示网页的网址或路径。如果省略，那么窗口就不显示任何文档

- 窗口名称可选，被打开窗口的名称

>  1.该名称由字母、数字和下划线字符组成。
>       2."_top"、"_blank"、"_self"具有特殊意义的名称。
>          _blank：在新窗口显示目标网页
>          _self：在当前窗口显示目标网页
>          _top：框架网页中在上部窗口中显示目标网页
>       3.相同 name 的窗口只能创建一个，要想创建多个窗口则 name 不能相同。
>      4.name 不能包含有空格。

- 窗口字符串

![windowstr](http://img.mukewang.com/52e3677900013d6a05020261.jpg)

sample

```html
<script type="text/javascript"> window.open('http://www.imooc.com','_blank','width=300,height=200,menubar=no,toolbar=no, status=no,scrollbars=yes')
</script>
```

打开http://www.imooc.com网站，大小为300px * 200px，无菜单，无工具栏，无状态栏，有滚动条窗口

#### window.close()

```javascript
window.close();//关闭本窗口
```

```html
<script type="text/javascript">
   var mywin=window.open('http://www.imooc.com'); //将新打的窗口对象，存储在变量mywin中
   mywin.close();
</script>
```

### 练习

> 1、新窗口打开时弹出确认框，是否打开
>
> ```
> 提示: 使用 if 判断确认框是否点击了确定，如点击弹出输入对话框，否则没有任何操作。
> ```
>
> 2、通过输入对话框，确定打开的网址，默认为 http：//www.imooc.com/
>
> 3、打开的窗口要求，宽400像素，高500像素，无菜单栏、无工具栏。

```html
<!DOCTYPE html>
<html>
 <head>
  <title> new document </title>  
  <meta http-equiv="Content-Type" content="text/html; charset=gbk"/>   
  <script type="text/javascript">  
    function openWindow(){
        var sta = confirm("是否打开窗口");
        if(sta == true){
            var url = prompt("请填写网址"," http：//www.imooc.com/")
            window.open('url','width=400,height=500,toolbar=no,menubar=no');
        }
    }   
  </script> 
 </head> 
 <body> 
	  <input type="button" value="新窗口打开网站" onclick="openWindow()" /> 
 </body>
</html>
```

### 变量

#### 声明

Javascript使用

```javascript
var x;
```

声明变量，可以存储数字、字符、字符串

#### 表达式

```javascript
var a = "goodbye";
var b = "verge";
var c = a + b;
alert(c);
```

结果是goodbyeverge

### 数组

#### 声明

```javascript
var myarr = new Array();
```

在括号内指定数组长度，虽然数组长度已经确定，但仍可以将数据储存在规定长度之外

#### 赋值

```javascript
var myarr = new Array(2);
myarr[0] = 1;
myarr[1] = 2;
```

实现同样的效果还有两种方法

```javascript
var myarr = new Array(1,2); 
```

```javascript
var myarr = [1,2];
```

可以使用未使用的下标，不断给数组添加成员

```javascript
myarr[2] = 3;
```

数组没有赋值直接输出的话会输出undefined

#### 数组长度

```javascript
myarr.length;
```

可以使用

```javascript
myarr.length = 一个数字;//改变数组长度
```

在你改变数组长度后，即使里面什么都没存，长度还是你改变后的。

如果，一个数组是这样var arr = [1,2,3] 长度为3，如果这是你进行 arr.length = 2,那么长度就会变成2，会把原来arr[2]的位置变为undefined

数组元素增加长度也会改变

```javascript
var myarr = [1,2,3];
alert(myarr.length);
myarr[3] = 4;
alert(myarr.length);
```

#### 二维数组

```javascript
myarr[][];
```

##### 定义

```javascript
var myarr=new Array();  //先声明一维 
for(var i=0;i<2;i++){   //一维长度为2
   myarr[i]=new Array();  //再声明二维 
   for(var j=0;j<3;j++){   //二维长度为3
   		myarr[i][j]=i+j;   // 赋值，每个数组元素的值为i+j
   }
}
```

```javascript
var myarr = [[0,1,2],[1,2,3]];//两行三列
```

```javascript
var myarr[0][1] = 123;
```

 ### 函数

#### 定义

```javascript
function funname(){
    do something;
}
```

#### 调用函数

1. 在\<script>标签内直接调用

```javascript
<script type="text/javascript">
	funname();    
</script>
```

2.  在HTML文件中调用，通过点击按钮调用定义好的函数

```HTML
<html>
    <head>
        <script type="text/javascript">
           function add2()
           {
                 sum = 5 + 6;
                 alert(sum);
           }
        </script>
    </head>
    <body>
        <form>
        	<input type="button" value="click it" onclick="add2()">  //按钮,onclick点击事件，直接写函数名
        </form>
    </body>
</html>
```

#### 含有参数的函数

```javascript
function add(x,y){
	document.write(x+y);
}
```

#### 有返回值的函数

```javascript
function add(x,y){
	document.write(x+y);
    return x+y;
}

res = add(x,y);
```

## 事件

事件是可以被Javascript侦测到的行为

![](http://img.mukewang.com/53e198540001b66404860353.jpg)

### onclick 鼠标点击事件

```html
<input type = "button" value = "点我调用add()" onclick="add()" >
```

### onload 加载事件

加载页面时会触发onload事件

可以理解为打开页面时会出发的事件

### onubload卸载事件

当用户刷新，关闭页面时触发的事件

### 其他事件使用方式大同小异

## 对象

### 什么是对象

JavaScript 中的所有事物都是对象，如:字符串、数值、数组、函数等，每个对象带有**属性**和**方法**。

**对象的属性：**反映该对象某些特定的性质的，如：字符串的长度、图像的长宽等；

**对象的方法：**能够在对象上执行的动作。例如，表单的“提交”(Submit)，时间的“获取”(getYear)等；**就是在对象的基础上再进行操作**



JavaScript 提供多个内建对象，比如 String、Date、Array 等等，使用对象前先定义，如下使用数组对象：

```
  var objectName =new Array();//使用new关键字定义对象
或者
  var objectName =[];
```

**访问对象属性的语法:**

```
objectName.propertyName
```

如使用 Array 对象的 length 属性来获得数组的长度：

```
var myarray=new Array(6);//定义数组对象
var myl=myarray.length;//访问数组长度length属性
```

**以上代码执行后，myl的值将是：6**

**访问对象的方法：**

```
objectName.methodName()
```

如使用string 对象的 toUpperCase() 方法来将文本转换为大写：

```
var mystr="Hello world!";//创建一个字符串
var request=mystr.toUpperCase(); //使用字符串对象方法
```

以上代码执行后，request的值是**：HELLO WORLD!**

### Date 日期对象

可以存储任意一个日期，可以精确到毫秒

```javascript
var date1 = new Date();
```

初始值为当前电脑系统世间

自定义初始值可以

```javascript
var date1 = new Date(2020,2,23);
var date1 = new Date('Apr 23,2020');
```

我们最好使用下面介绍的“方法”来严格定义时间。

**访问方法语法：**“<日期对象>.<方法>”

Date对象中处理时间和日期的常用方法：

![](http://img.mukewang.com/555c650d0001ae7b04180297.jpg)

#### 返回/设置年份的方法

```javascript
var mydate = new Date();
mydate.setFullYear(20);//不同浏览器结果不同，有可能是20或0020
var myyear = mydate.getFullYear();
```

#### 返回星期方法

```javascript
mydate.getDay()//返回的是0-6的一个数字，0是周日
```

#### 返回/设置时间的方法

**get/setTime()** 返回/设置时间，单位毫秒数，计算从 1970 年 1 月 1 日零时到日期对象所指的日期的毫秒数。

时间加一小时就是

```javascript
mydate.setTime(mydate.getTime()+60*60*1000);
```

### String 字符串对象

```javascript
var str = "jinxuyang";
var srtlen = str.length;//返回字符串长度
var STR = str.toUpperCase();//转换为大写,toLowerCase()转换为小写
```

#### charAT() 返回指定位置的字符

```javascript
stringOject.charAt(index)
```

参数index即字符在字符串中的下标

返回一个字符长度为1的字符串

#### indexOf() 返回指定的字符串首次出现的位置

```javascript
stringObject.indexOf(substring, startpos)
```

![](http://img.mukewang.com/53853d4200019feb04920149.jpg)

返回需要检索的字符串的第一个字符第一次出现的位置，如果没有则返回-1

#### split() 字符串分割

```javascript
stringObject.split(separator,limit)
```

![](http://img.mukewang.com/532bee4800014c0404230108.jpg)

如果把空字符串 ("") 用作 separator，那么 stringObject 中的每个字符之间都会被分割。

若达到分割的次数之后，后面分割出来的字串就不会输出，或者说到达分割的次数之后就不再分割

#### substring() 提取字符串

```javascript
stringObject.substring(startPos,stopPos)
```

![](http://img.mukewang.com/532bf1bb000151af04450082.jpg)

startPos和stopPos可以理解为字符串数组开始和结束的下标，两个参数相等的话就返回一个空串，如果stop比start大会先交换这两个参数

stopPos

假设一个字符串是abcdefg那么substring(0,4)提取出来的结果是abcd，到stopPos结束，并不包括stopPos

#### substr() 提取指定数目字符串

```java
stringObject.substr(startPos,length)
```

![](http://img.mukewang.com/532bf2e00001105305100098.jpg)

**注意：**如果参数startPos是负数，从字符串的尾部开始算起的位置。也就是说，-1 指字符串中最后一个字符，-2 指倒数第二个字符，以此类推。

如果startPos为负数且绝对值大于字符串长度，startPos为0

#### search() 检索字符串中指定的子字符串（正则）

```javascript
stringObject.search(reg/str)
```

返回第一个匹配结果的index，查找不到返回-1

search()不执行全局匹配，总是从字符串的开始进行检索

#### match() 检索字符串找到一个或多个与RegExp匹配的文本

```javascript
str.match(regexp)
```

参数

- `regexp`

  一个[正则表达式](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)对象。如果传入一个非正则表达式对象，则会隐式地使用 `new RegExp(obj)` 将其转换为一个 [`RegExp`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/RegExp) 。如果你没有给出任何参数并直接使用match() 方法 ，你将会得到一 个包含空字符串的 [`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Array) ：[""] 。

返回值

- 如果使用g标志，则将返回与完整正则表达式匹配的所有结果，但不会返回捕获组。
- 如果未使用g标志，则仅返回第一个完整匹配及其相关的捕获组（`Array`）。 在这种情况下，返回的项目将具有如下所述的其他属性。

附加属性

如上所述，匹配的结果包含如下所述的附加特性。

- `groups`: 一个捕获组数组 或 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined)（如果没有定义命名捕获组）。
- `index`: 匹配的结果的开始位置
- `input`: 搜索的字符串.

描述

如果正则表达式不包含 `g `标志，`str.match()` 将返回与 [`RegExp.exec()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp/exec). 相同的结果。

#### replace() 替换字符串中指定子字符串或正则匹配的结果

```javascript
str.replace(regexp,"替换成的内容")
str.replace("要替换的内容"，"替换成的内容")
```



### Math 对象

**注意：**Math 对象是一个固有的对象，无需创建它，直接把 Math 作为对象使用就可以调用其所有属性和方法。这是它与Date,String对象的区别。

Math 对象属性

![](http://img.mukewang.com/532fe7cf0001e7b505170269.jpg)

Math对象方法

![](http://img.mukewang.com/532fe841000174db05160622.jpg)

### Array 数组对象

#### concat() 数组连接

```javascript
arrayObject.concat(array1,array2,...,arrayN)
```

返回一个连接后的数组，不会改变原来的数组

```javascript
var mya = new Array(3);
  mya[0] = "1";
  mya[1] = "2";
  mya[2] = "3";
  document.write(mya.concat(4,5));
```

可以这样写直接给数组后面在加上4，5，**注意并不改变mya数组**

#### join() 指定分隔符连接数组元素

```javascript
arrayObject.join(分隔符)
```

若参数留空则默认使用，分开

#### reverse() 颠倒数组元素顺序

```javascript
arrayObject.reverse()
```

**注意：**该方法会改变原来的数组，而不会创建新的数组。

#### slice() 选定元素

```javascript
arrayObject.slice(start,end)
```

![](http://img.mukewang.com/533299680001637b05160145.jpg)

返回一个新的数组，包含从 start 到 end （不包括该元素）的 arrayObject 中的元素

#### sort() 数组排序

**sort()**方法使数组中的元素按照一定的顺序排列。

**语法:**

```
arrayObject.sort(方法函数)
```

**参数说明：**

![img](http://img.mukewang.com/53329a2a000127f705170060.jpg)

1.如果不指定<方法函数>，则按unicode码顺序排列。

2.如果指定<方法函数>，则按<方法函数>所指定的排序方法排序。

```
myArray.sort(compareFunction);
```

如果没有指明 `compareFunction` ，那么元素会按照转换为的字符串的诸个字符的Unicode位点进行排序。例如 "Banana" 会被排列到 "cherry" 之前。当数字按由小到大排序时，9 出现在 80 之前，但因为（没有指明 `compareFunction`），比较的数字会先被转换为字符串，所以在Unicode顺序上 "80" 要比 "9" 要靠前。

如果指明了 `compareFunction` ，那么数组会按照调用该函数的返回值排序。即 a 和 b 是两个将要被比较的元素：

- 如果 `compareFunction(a, b)` 小于 0 ，那么 a 会被排列到 b 之前；

- 如果 `compareFunction(a, b)` 等于 0 ， a 和 b 的相对位置不变。备注： ECMAScript 标准并不保证这一行为，而且也不是所有浏览器都会遵守（例如 Mozilla 在 2003 年之前的版本）；

- 如果 `compareFunction(a, b)` 大于 0 ， b 会被排列到 a 之前。
- `compareFunction(a, b)` 必须总是对相同的输入返回相同的比较结果，否则排序的结果将是不确定的。

所以，比较函数格式如下：

```js
function compare(a, b) {
  if (a < b ) {           // 按某种排序标准进行比较, a 小于 b
    return -1;
  }
  if (a > b ) {
    return 1;
  }
  // a must be equal to b
  return 0;
}
```

具体实现

```javascript
<script type="text/javascript">
  function sortNum(a,b) {
  return a - b;
 //升序，如降序，把“a - b”该成“b - a”
}
 var myarr = new Array("80","16","50","6","100","1");
  document.write(myarr + "<br>");
  document.write(myarr.sort(sortNum));
</script>
```

**运行结果：**

```
80,16,50,6,100,1
1,6,16,50,80,100
```

### window对象

![](http://img.mukewang.com/535483720001a54506670563.jpg)

### JavaScript定时器

![](http://img.mukewang.com/56976e1700014fc504090143.jpg)

#### setInterval()

载入页面后每隔指定时间执行代码

```javascript
setInterval(代码，交互时间)
```

代码：要执行的代码

交互时间：周期执行的时间间隔，单位毫秒

返回值：是一个值，可以把这个值传给clearInterval()从而取消setInterval()的执行

**调用函数格式(**假设有一个clock()函数**):**

```
setInterval("clock()",1000)
或
setInterval(clock,1000)
```

#### clearInterval()

```javascript
clearInterval(id_of_setInterval)
```

#### setTimeout()

```javascript
setTimeout(代码，延迟时间);
```

延迟时间到后执行代码

#### clearTimeout()

与clearInterval()相似

### history对象

history对象记录了用户曾经浏览过的页面(URL)，并可以实现浏览器前进与后退相似导航的功能。

**注意:从\**窗口\**被打开的那一刻开始记录，每个浏览器窗口、每个标签页乃至每个框架，都有自己的history对象与特定的window对象关联。**

**语法：**

```
window.history.[属性|方法]
```

**注意：**window可以省略。

**History 对象属性**

[![img](http://img.mukewang.com/53548c030001759e05840068.jpg)](http://img.mukewang.com/53548c030001759e05840068.jpg)

**History 对象方法**

**[![img](http://img.mukewang.com/53548c200001228206210123.jpg)](http://img.mukewang.com/53548c200001228206210123.jpg)**

使用length属性，当前窗口的浏览历史总长度，**代码如下：**

```
<script type="text/javascript">
  var HL = window.history.length;
  document.write(HL);
</script>
```

#### go()



语法：

```
window.history.go(number);
```

**参数：**

[![img](http://img.mukewang.com/5354947e00011a9a06490153.jpg)](http://img.mukewang.com/5354947e00011a9a06490153.jpg)





### location对象

location用于获取或设置窗体的URL，并且可以用于解析URL。

**语法:**

```
location.[属性|方法]
```

**location对象属性图示:**

[![img](http://img.mukewang.com/53605c5a0001b26909900216.jpg)](http://img.mukewang.com/53605c5a0001b26909900216.jpg)

**location 对象属性：**

**[![img](http://img.mukewang.com/5354b1d00001c4ec06220271.jpg)](http://img.mukewang.com/5354b1d00001c4ec06220271.jpg)**

**location 对象方法:**

**[![img](http://img.mukewang.com/5354b1eb00016a2405170126.jpg)](http://img.mukewang.com/5354b1eb00016a2405170126.jpg)**



### navigator对象

![](http://img.mukewang.com/5354cff70001428b06880190.jpg)



## DOM

### 什么是DOM

参考<a href="https://www.runoob.com/htmldom/htmldom-nodes.html">这个</a>

### 获取元素

#### getElementById

```javascript
document.getElementById("id");
```

**获取的元素是一个对象，想要对这个元素操作还需要其他东西**

```html
<p id="1">text</p>
<script type="text/javascript">
	var str = document.getElementById("1");
    documen.write(str);
</script>
```

这样是**错误**的

#### getElementsByName

Element比上面那个多了个s

```javascript
document.getElementsByName();
```

由于name属性可能是不唯一的，因此返回值是元素数组，同样也有length属性

#### getElementsByTagName

```
document.getElementsByTagName(Tagname)
```

 Tagname是标签的名称，如p、a、img等标签名

返回值也是数组也有length

#### getAttribute()

通过元素结点的属性名获取属性的值

```
elementNode.getAttribute(name)
```

### 操作元素



#### setAttribute()

```
elementNode.setAttribute(name,value)
```

1.name: 要设置的属性名。

2.value: 要设置的属性值

**注意：**

1.把指定的属性设置为指定的值。如果不存在具有指定名称的属性，该方法将创建一个新属性。

#### innerHTML

```javascript
object.innerHTML
```

object是获取的元素

```html
<p id="1">text</p>
<script type="text/javascript">
	var str = document.getElementById("1");
    documen.write(str.innerHTML);
</script>
```

这样就对了

#### 改变HTML样式

```html
object.style.property=new style;
```

object是获取的元素

property

![111](http://img.mukewang.com/52e4d4240001dd6c04850229.jpg)

```html
<p id="1">text</p>
<script type="text/javascript">
	var str = document.getElementById("1");
    document.write(str.innerHTML);
    str.style.color="pink";//变粉
</script>
```

#### 显示和隐藏

```javascript
object.style.display = "value";
```

value的值

![display](http://img.mukewang.com/52e4dba5000179da04110095.jpg)

```html
<p id="1">text</p>
<script type="text/javascript">
	var str = document.getElementById("1");
    document.write(str.innerHTML);
    str.style.color="pink";//变粉
    str.style.display="none";//隐藏
</script>
```

#### 控制类名

```javascript
object.className = "classname";
```

获取元素class属性

为元素指定一个css样式

```html
<script type="text/javascript">
	var str = document.getElementById("1");
    document.write(str.innerHTML);
    str.style.color="pink";//变粉
    str.style.display="none";//隐藏
    str.classNme = "hahaha"//改变元素类名
</script>
<style type="text/css">
    .ha{color:pink;}
    .hahaha{color:blue;}
</style>
<p id="1" class="ha">text</p>

```

### 结点属性

在文档对象模型 (DOM) 中，每个节点都是一个对象。DOM 节点有三个重要的属性 ：

1. nodeName : 节点的名称

2. nodeValue ：节点的值

3. nodeType ：节点的类型

**一、nodeName 属性:** 节点的名称，是只读的。

1. 元素节点的 nodeName 与标签名相同
2. 属性节点的 nodeName 是属性的名称
3. 文本节点的 nodeName 永远是 #text
4. 文档节点的 nodeName 永远是 #document

**二、nodeValue 属性：**节点的值

1. 元素节点的 nodeValue 是 undefined 或 null
2. 文本节点的 nodeValue 是文本自身
3. 属性节点的 nodeValue 是属性的值

**三、nodeType 属性:** 节点的类型，是只读的。以下常用的几种结点类型:

**元素类型   节点类型**
 元素      1
 属性      2
 文本      3
 注释      8
 文档      9

### 访问结点

#### 访问子节点childNodes

```
elementNode.childNodes
```

返回值是所有子节点的列表，可以看作数组，没有子节点则返回一个不包含结点的列表

##### 访问子节点的第一项和最后一项

一、**`firstChild `**属性返回‘childNodes’数组的第一个子节点。如果选定的节点没有子节点，则该属性返回 NULL。

**语法：**

```
node.firstChild
```

**说明：**与elementNode.childNodes[0]是同样的效果。 

二、**` lastChild`** 属性返回‘childNodes’数组的最后一个子节点。如果选定的节点没有子节点，则该属性返回 NULL。

**语法：**

```
node.lastChild
```

**说明：**与elementNode.childNodes[elementNode.childNodes.length-1]是同样的效果。 

#### 访问父节点parentNode

```
elementNode.parentNode
```

一个结点只能有一个父结点

#### 访问兄弟结点

1. nextSibling 属性可返回某个节点之后紧跟的节点（处于同一树层级中）。

**语法：**

```
nodeObject.nextSibling
```

**说明：**如果无此节点，则该属性返回 null。

2. previousSibling 属性可返回某个节点之前紧跟的节点（处于同一树层级中）。

**语法：**

```
nodeObject.previousSibling  
```

**说明：**如果无此节点，则该属性返回 null。

### 插入结点

#### appendChild()

```
node.appendChild(newnode)
```

在指定节点的**最后一个子节点**列表之后添加一个新的子节点。

这里的newnode需要使用`document.createElement("标签")`来创建

#### insertBefore()

方法在参考节点之前插入一个拥有指定父节点的子节点

```javascript
node.insertBefore(newnode,node);
```

newnode: 要插入的新节点。

node: 指定此节点前插入节点。

而所谓的“拥有指定父节点”，就是指被参照的节点的父节点就是调用insertBefore方法的节点。

如果给定的子节点是对文档中现有节点的引用，insertBefore()会将其从当前位置移动到新位置。

如果给定的子节点是DocumentFragment，那么DocumentFragment的全部内容将被移动到指定父节点的子节点列表中。

### 删除节点

#### removeChild()

```
let oldChild = node.removeChild(child);

//OR

element.removeChild(child);
```

- `child` 是要移除的那个子节点.
- `node` 是`child`的父节点.
- oldChild保存对删除的子节点的引用. `oldChild` === `child`.

被移除的这个子节点仍然存在于内存中,只是没有添加到当前文档的DOM树中,因此,你还可以把这个节点重新添加回文档中,当然,实现要用另外一个变量比如`上例中的oldChild`来保存这个节点的引用. 如果使用上述语法中的第二种方法, 即没有使用 oldChild 来保存对这个节点的引用, 则认为被移除的节点已经是无用的, 在短时间内将会被[内存管理](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Memory_Management)回收.

#### replaceChild()

replaceChild 实现子节点(对象)的替换。返回被替换对象的引用。 

**语法：**

```
node.replaceChild (newnode,oldnew ) 
```

**参数：**

newnode : 必需，用于替换 oldnew 的对象。 
oldnew : 必需，被 newnode 替换的对象。