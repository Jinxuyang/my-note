---
title: JDBC学习笔记
date: 2020-05-30 01:04:43
tags:
---

### JDBC是啥

JDBC(Java DataBase connectivity)(Java 数据库连接)

由SUN公司定义的一系列操作关系型数据库的接口，后由数据库厂商对接口进行实现，提供数据库驱动jar包

<!--more-->

##  使用

### 导入jar包

 

### 加载驱动

```java
Class.forName("com.mysql.jdbc.Driver");//把com.mysql.jdbc.Driver这个字节码文件加载到内存里
```

### 连接数据库

`host = jdbc:mysql://host /db`

```java
Connection conn = DriverManager.getConnection(host,username,passwd);
```

连接时发现报错

> The server time zone value '�й���׼ʱ��' is unrecognized or represents more than one time zone. You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.

解决方法

host后加上`?serverTimezone=UTC`

## 对象详解

1. DriverManage驱动管理
   - 注册驱动
     - MySQL5以后可以自动注册
   - 获取数据库连接
     - static getConnection(String url,String user,String password)
     - url = jdbc:mysql://ip地址:端口号/数据库名
2. Connection数据库连接对象
   1. 获取执行sql语句的对象
      -  Statement CreateStatement(String sql)
      - Prestatement prepareStatement(String sql)
   2. 管理事务
3. Statement执行sql语句
   1. 执行sql
      1. int executeUpdate(String sql)：执行DML(insert,update,delete)，DDL(create,alter,drop)
         - 返回值是操作后影响的行数
         - 可以用来判断成功与否
      2. ResultSet executeQuery(String sql):执行DQL(select)
4. ResultSet查询结果的封装
   - boolean next()游标向下移动一行(默认指向表头)，判断是否有数据
   - getXxx("列名/列的编号，从1开始")获取数据Xxx代表类型
5. PreparedStatement
   1. SQL注入问题:在拼接sql‘语句时，有一些特殊的词参与拼接会造成安全问题
   2. 使用PreparedStatem对象解决问题
      1. 定义sql语句时使用`?`作为占位符，替换参数
      2. 使用setXxx(？的编号从1开始，参数的值)
      3. 执行sql语句时不再需要传参

## Spring JDBC

Spring框架提供的JDBC简单封装

提供一个JDBCTemplate

### 使用

1. 导入jar包
2. 创建JdbcTemplate，依赖于DataSource
   1. JdbcTemplate template = new JdbcTemplate
3. 调用JdbcTemplate的方法完成CRUD操作
   1. update()增删改
   2. queryForMap()将查询结果封装为map集合
   3. queryForList()将查询结果封装为list集合
   4. query()查询结果，将查询结果封装为JavaBean对象
   5. queryForObject()将查询结果封装为对象

