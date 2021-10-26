---
title: MyBatis学习笔记
date: 2020-06-22 10:21:37
tags:
	- mybatis
	- 学习笔记
	- java
categories: 
	- 框架
---

[TOC]





## 环境搭建

1. 创建实体类和实体类的接口
   1. User
   2. IUserDao
   
   <!--more-->
   
2. 配置主配置文件

   SqlMapConfig.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <!-- 配置 mybatis 的环境 default属性没有固定的值 -->
       <environments default="mysql">
           <!-- 配置 mysql 的环境 这里的ID需要和environments标签中的default属性相同-->
           <environment id="mysql">
               <!-- 配置事务的类型 -->
               <transactionManager type="JDBC"></transactionManager>
               <!-- 配置连接数据库的信息：用的是数据源(连接池) -->
               <dataSource type="POOLED">
                   <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://121.36.19.47:3306/mybatis_learn"/>
                   <property name="username" value="root"/>
                   <property name="password" value=""/>
               </dataSource>
           </environment>
       </environments>
       <!-- 告知 mybatis 映射配置的位置 每个Dao独立的配置文件-->
       <mappers>
           <mapper resource="dao/IUserDao.xml"/>
       </mappers>
   </configuration>
   
   ```

   

3. 创建映射配置文件

   1. IUserDao.xml

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <!DOCTYPE mapper
              PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
              "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
      <!-- namespace写类的全限定类名 -->
      <mapper namespace="dao.IUserDao">
          <!-- 配置查询所有操作 ID填写方法名 resultType填写要封装成的JavaBean-->
          <select id="findAll" resultType="domain.User">
              select * from user
          </select>
      </mapper>
      ```

      

**注意**：

1. MyBatis的映射配置文件位置必须和Dao接口的包结构相同
2. 映射配置文件的mapper标签的namespace属性取值必须是Dao接口的全限定类名
3. 映射配置文件的操作配置，id属性的取值必须是dao接口的方法名



​	遵从这三点，在开发中就无需实现IUserDao的类，MyBatis代替我们完成



## 使用

1. 读取配置文件

   ```java
   InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
   ```

   

2. 创建SqlSessionFactory工厂

   ```java
   SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
   SqlSessionFactory factory = builder.build(in);
   ```

   

3. 创建SqlSession对象

   ```java
   SqlSession session = factory.openSession();
   ```

   

4. 创建Dao接口的代理对象

   ```java
   IUserDao userDao = session.getMapper(IUserDao.class); 
   ```

5. 释放资源

   ```java
   session.close();
   in.close();
   ```

   

## 注解使用

1. 省略映射配置文件

2. 在要实现的接口上写

   ```java
   @Select("Sql语句")
   ```

3. 主配置文件\<mapper>标签下使用class属性指定被注解的Dao全限定类名

   ```xml
   <mappers>
   	<mapper class="dao.IUserDao" />
   </mappers>
   ```

   

## CURD

### 增

#### 配置文件写法

```xml
<insert id="addUser" parameterType="domain.User">
	INSERT INTO user(username,sex,address) VALUS(#{username},#{sex},#{address});
</insert>

```

#### 注解写法



 ```sql
@Insert("INSERT INTO user(username,sex,address) VALUS(#{username},#{sex},#{address})")
 ```



需要向接口中传入一个User对象

最后需要commit

```java
session.commit();
```

#### 获取插入数据的ID

```xml
<insert id="addUser" parameterType="domain.User">
    <selectKey keyProperty="id" keyColumn=”id“ resultType="int" order="AFTER">
        SELECT last_insert_id();
    </selectKey>
	INSERT INTO user(username,sex,address) VALUS(#{username},#{sex},#{address});
</insert>

```

keyProperty表示返回值的名称 

order表示执行该命令的时间，AFTER表示插入之后再执行



### 改、删

与增加操作类似只需要改变标签名称或注解的@Xxx

```xml
<update id="Xxx" parameterType="xx.xx">
	Sql语句
</update>

<delete id="Xxx" parameterType="xx.xx">
	Sql语句
</delete>
```

```java
@Update("....")
@Delete("....")
```

### 查



### 获取用户总记录的条数

```xml
<select id="findTotal" resultType="int">
	SELECT count(id) FROM user;
</select>
```



## OGNL表达式

Object Graphic Navigation Language

## 连接池与事务控制

### MyBatis中的连接池

mybatis中提供了3种方式配置

在SqlMapConfig.xml中的dataSource标签中，type属性就是配置采用何种连接池的方式

- POOLED		采用传统javax.sql.DataSource规范中的连接池
- UNPOOLED  虽然实现了javax.sql.DataSource，但没有使用池的思想
- JNDI

### 事务

可以在创造SqlSession对象时，给OpenSession中传一个true，就可以实现自动提交

## 动态SQL语句

### \<if>标签

```xml
<select id="findUser" resultType="domain.User" parameterType="domain.User">
    SELECT * FROM user WHERE 1=1
    <if test="username != NULL">
    	AND username = #{username}
    </if>
    <if test="...">
    	AND ...
    </if>
</select>
```

### \<where>

```xml
<select id="findUser" resultType="domain.User" parameterType="domain.User">
    SELECT * FROM user
    <where>
    	<if test="username != NULL">
    	AND username = #{username}
    </if>
    <if test="...">
    	AND ...
    </if>
    </where>
</select>
```

## 多表查询

I don't have a favorite singer, but a film soundtrack composer whose name is hans zimmer is my favorite composer. I know him from a film called Interstellar. At the same time, Interstellar is also my favorite film. It is because of this excellent OST that there are such excellent film。

Let's enjoy it together