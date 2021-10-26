---
title: PHP学习笔记
date: 2020-04-29 00:17:56
tags:
	- php
categories: 
	- php
---

[TOC]



## 一些小细节

php代码需要写在`<?php ?>`之间

echo用来输出

php连接两个字符串使用`.`而不是`+`

php每条语句都以;结尾

<!--more-->

`//`表示注释

## PHP的新东西

### 数组

```php
$x = array();
```

PHP有两种数组：索引数组、关联数组。

#### 索引数组

索引数组是指数组的键是整数的数组，并且键的整数顺序是从0开始，依次类推。

```php
$fruit = array("苹果","香蕉","菠萝");
```

可以使用`print_r($fruit);`语句输出数组键及对应的值

##### 赋值

`$arr[0]='苹果';`

`array('0'=>'苹果');`

`array("苹果","香蕉");`

##### 读取

`$arr["0"];`/`$arr[0];`

 #### 关联数组

关联数组是指数组的键是字符串的数组

```php
$fruit = array(
    'apple'=>"苹果",
    'banana'=>"香蕉",
    'pineapple'=>"菠萝"
); 
```

可以使用`print_r($fruit);`语句输出数组键及对应的值。

##### 赋值

`$arr['apple']='苹果';`

`array('apple'=>'苹果');`

##### 读取

`$fruit['banana'];`

### 类与对象

类是一类东西的结构描述，而对象则是一类东西的一个具体实例

定义一个类

```php
//定义一个类
class Car {
    //定义属性
    public $name = '汽车';

    //定义方法
    public function getName() {
        //方法内部可以使用$this伪变量调用对象的属性或者方法
        return $this->name;
    }
}
```

对象通过new关键字进行实例化：

```php
$car = new Car();
//也可以采用变量来创建
$className = 'Car';
$car = new $className();
```

#### 类的属性

在类中定义的变量称之为属性

属性声明是由关键字 public，protected 或者 private 开头，后面跟一个普通的变量声明来组成

public：公开的
protected：受保护的
private：私有的

```php
class Car {
    //定义公共属性
    public $name = '汽车';
    //定义受保护的属性
    protected $corlor = '白色';
    //定义私有属性
    private $price = '100000';
}
```

默认都为public，外部可以访问。一般通过->对象操作符来访问对象的属性或者方法，对于静态属性则使用::双冒号进行访问。当在类成员方法内部调用的时候，可以使用$this伪变量调用当前对象的属性。

```php
$car = new Car();
echo $car->name;   //调用对象的属性
echo $car->color;  //错误 受保护的属性不允许外部调用
echo $car->price; //错误 私有属性不允许外部调用
```

受保护的属性与私有属性不允许外部调用，在类的成员方法内部是可以调用的。

```php
class Car{
    private $price = '1000';
    public function getPrice() {
        return $this->price; //内部访问私有属性
    }
}
```

#### 类的方法

方法就是在类中的function

的方法也具有public，protected 以及 private 的访问控制。

```php
class Car {
    public function getName() {
        return '汽车';
    }
}
$car = new Car();
echo $car->getName();
```

使用关键字static修饰的，称之为静态方法，静态方法不需要实例化对象，可以通过类名直接调用，操作符为双冒号::

```php
class Car {
    public static function getName() {
        return '汽车';
    }
}
echo Car::getName(); //结果为“汽车”
```

#### 构造函数和析构函数

PHP5可以在类中使用**__construct()**定义一个构造函数，具有构造函数的类，会在每次对象创建的时候调用该函数

```php
class Car {
   function __construct() {
       print "构造函数被调用\n";
   }
}
$car = new Car(); //实例化的时候 会自动调用构造函数__construct，这里会输出一个字符串
```

在子类中如果定义了__construct则不会调用父类的__construct，如果需要同时调用父类的构造函数，需要使用parent::__construct()显式的调用



PHP5支持析构函数，使用**__destruct()**进行定义，析构函数指的是当某个对象的所有引用被删除，或者对象被显式的销毁时会执行的函数

```php
class Car {
   function __construct() {
       print "构造函数被调用 \n";
   }
   function __destruct() {
       print "析构函数被调用 \n";
   }
}
$car = new Car(); //实例化时会调用构造函数
echo '使用后，准备销毁car对象 \n';
unset($car); //销毁时会调用析构函数
```

#### 静态关键字

静态属性与方法可以在不实例化类的情况下调用，直接使用`类名::方法名`的方式进行调用。静态属性**不允许**对象使用->操作符调用。

```php
class Car {
    private static $speed = 10;  
    public static function getSpeed() {
        return self::$speed;
    }
}
echo Car::getSpeed();  //调用静态方法
```

静态方法也可以通过变量来进行动态调用

```php
$func = 'getSpeed';
$className = 'Car';
echo $className::$func();  //动态调用静态方法
```

静态方法中，$this伪变量不允许使用。可以使用self，parent，static在内部调用静态方法与属性。

```php
class Car {
    private static $speed = 10;
    public static function getSpeed() {
        return self::$speed;
    }   
    public static function speedUp() {
        return self::$speed+=10;
    }
}
class BigCar extends Car {
    public static function start() {
        parent::speedUp();
    }
}
BigCar::start();
echo BigCar::getSpeed();
```

#### 访问控制

访问控制通过关键字public，protected和private来实现。

被定义为公有的类成员可以在任何地方被访问。

被定义为受保护的类成员则可以被其自身以及其子类和父类访问。

被定义为私有的类成员则只能被其定义所在的类访问。

默认为公有

```php
class Car {
    private function __construct() {
        echo 'object create';
    }

    private static $_object = null;
    public static function getInstance() {
        if (empty(self::$_object)) {
            self::$_object = new Car(); //内部方法可以调用私有方法，因此这里可以创建对象
        }
        return self::$_object;
    }
}
//$car = new Car(); //这里不允许直接实例化对象
$car = Car::getInstance(); //通过静态方法来获得一个实例
```

#### 对象继承

```php
class Truck extends Car{
    public function speedUp() {
        $this->speed += 50;
        return $this->speed;
    }
}
```

使用extends

#### 重载

### cookie

#### 设置cookie

### 文件系统

### 异常处理

```php
try{
            //可能出现错误或异常的代码
            //catch表示捕获，Exception是php已定义好的异常类
} catch(Exception $e){
            //对异常处理，方法：
                //1、自己处理
                //2、不处理，将其再次抛出
}
```

#### 抛出异常

```php
//创建可抛出一个异常的函数
function checkNum($number){
     if($number>1){
         throw new Exception("异常提示-数字必须小于等于1");
     }
     return true;
 }
 
//在 "try" 代码块中触发异常
 try{
     checkNum(2);
     //如果异常被抛出，那么下面一行代码将不会被输出
     echo '如果能看到这个提示，说明你的数字小于等于1';
 }catch(Exception $e){
     //捕获异常
     echo '捕获异常: ' .$e->getMessage();
 }
```

#### 异常处理类

PHP具有很多异常处理类，其中Exception是所有异常处理的基类。

Exception具有几个基本属性与方法，其中包括了：

message 异常消息内容
code 异常代码
file 抛出异常的文件名
line 抛出异常在该文件的行数

其中常用的方法有：

getTrace 获取异常追踪信息
getTraceAsString 获取异常追踪信息的字符串
getMessage 获取出错信息

#### 捕获异常信息

```php
try {
    throw new Exception('wrong');
} catch(Exception $ex) {
    echo 'Error:'.$ex->getMessage().'<br>';
    echo $ex->getTraceAsString().'<br>';
}
echo '异常处理后，继续执行其他代码';
```

#### 将错误信息写入错误日志

```php
try {
    throw new Exception('wrong');
} catch(Exception $ex) {
    $msg = 'Error:'.$ex->getMessage()."\n";
    $msg.= $ex->getTraceAsString()."\n";
    $msg.= '异常行号：'.$ex->getLine()."\n";
    $msg.= '所在文件：'.$ex->getFile()."\n";
    //将异常信息记录到日志中
file_put_contents('error.log', $msg);
}
```

### PHP数据库

```php
$host = '127.0.0.1';
$user = 'code1';
$pass = '';
mysql_connect($host,$user,$pass);//连接数据库
mysql_select_db('code1');//选择数据库
mysql_query("set names 'utf8'");//设置字符编码
```

#### 查询

在数据库建立连接以后就可以进行查询，采用mysql_query加sql语句的形式向数据库发送查询指令。

```php
$res = mysql_query('select * from user limit 1');
```

对于查询类的语句会返回一个资源句柄（resource），可以通过该资源获取查询结果集中的数据。

```php
$row = mysql_fetch_array($res);
var_dump($row);
```

#### 插入

```php
mysql_query("insert into user(name, age, class) values('李四', 18, '高三一班')"); //执行插入语句
```

在mysql中，执行插入语句以后，可以得到自增的主键id,通过PHP的mysql_insert_id函数可以获取该id。

```
$uid = mysql_insert_id();
```

#### 取得数据查询结果

