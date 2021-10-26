---
title: 数据库连接池
date: 2020-06-01 19:46:07
tags:
---

## 什么是数据库连接池

数据库连接池就是一个存放数据库连接的容器

当系统初始化完成后，容器就被创建，容器中会申请一些连接对象，当用户来访问数据时，从容器中获取连接对象，用户访问完后，将连接对象归还给容器

<!--more-->

## 实现

1. 接口：DataSource
   1. 方法
      - 获取连接：getConnection()
      - 归还连接：Connection.close()
        - 如果连接是从数据库连接池中获得的就归还给连接池，而不是关闭连接
   2. 由数据库厂商实现\
      1. 数据库连接池技术
         1. C3P0
         2. Druid

## C3P0

### 使用

1. 导入jar包
2. 定义配置文件：
   1. c3p0.properties/c3p0-config.xml
   2. 放在src文件夹下
   3. 创建核心对象 数据库连接池对象 ComboPooledDataSource( )

## Druid

### 使用

1. 导入jar包
2. 配置文件
   1. xxx.properties
   2. 可以叫任意名称，放在任意目录下
3. 加载配置文件
   1. Properties
4. 数据库连接池对象 DruidDataSourceFactory 
   1. 参数：字节流对象
   2. 方法：createDataSource获取一个DataSource对象



```java
private static DataSource dataSource;

    static {

        try {
            Properties pro = new Properties();
            pro.load(DruidUtils.class.getClassLoader().getResourceAsStream("druid.properties"));
            dataSource = DruidDataSourceFactory.createDataSource(pro);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```


