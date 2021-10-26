---
title: C语言中struct用法
date: 2020-02-13 15:27:39
tags:
	- C
	- 学习笔记
categories: 
	- C
---

## 结构

### 声明结构类型

```c
struct date{
	int year;
	int month;
	int day;
};//声明一种结构，结构内包含year，month，day三个成员

struct date today;//定义一个变量today，这个变量的类型是date
```

**在函数内声明的结构只可以在函数内使用**

<!--more-->

### 声明结构的三种形式

```c
struct point {
	int x;
	int y;
};

struct point p1,p2;
```

```c
struct {
	int x;
	int y;
}p1,p2;//只定义了两个变量
```

```c
struct point {
	int x;
	int y;
}p1,p2;
```



### 结构的初始化

```c
struct date today = {2020,02,13};
struct date yesterday = {.month = 2, .year = 2020};
```



### 结构成员

结构使用"."来访问成员

today.year = xxxx;

### 结构运算

```c
p1 = (struct point){5,10};//相当于怕p1.x = 5;p2.y = 10; 
p1 = p2; //相当于p1.x = p2.x;p1.y = p2.y;
```

### 结构指针

- 与数组不同，结构的变量名并不是结构变量的地址，必须使用&
- struct date *pDate = &today;

## 结构与函数

### 结构作为函数参数

- 整个结构可以作为参数的值传入函数
- 实际上进行的操作是在函数内新疆一个结构变量，并复制调用者的结构的值
- 也可返回一个结构

## typedef

```c
typedef struct{
    int x;
    int y;
}point;
```

h表示声明一个结构，结构名为point