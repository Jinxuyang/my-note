---
title: Spring学习笔记
date: 2020-05-30 23:38:55
tags:
	- spring
	- 学习笔记
categories: 
	- spring
---



[TOC]



## IoC(控制反转)

将对象的创建交给框架

目的：降低程序间的耦合

<!--more-->

### 如何使用

1. Maven项目引入一下依赖

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.2.7.RELEASE</version>
</dependency>
```

2. 在项目的src文件下创建bean.xml(可以是任何名称文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   </beans>
   
   ```

3. 配置bean标签

   ```xml
   <!-- bean标签：用于配置让spring创建对象，并且存入ioc容器之中 -->
   <!-- id属性：对象的唯一标识。 class属性：指定要创建对象的全限定类名 -->
   <bean id="xxx" class="com.xxx.xxx.xxx"></bean>
   ```

4. 创建对象

   ```java
   public static void main(String[] args) { 
       //1.使用ApplicationContext接口获取spring容器 
       ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml"); 
       //2.根据bean的id获取对象 
       Object ObjectName = (Object) ac.getBean("bean.xml里的id"); 
   	} 
   }
   ```

#### BeanFactory与ApplicationContext

BeanFactory是顶层接口

ApplicationContext是BeanFactory的子接口

不同：对象的创建时间不同

- BeanFactory 什么时候用什么时候创建
- ApplicationContext默认情况下读取完配置文件之后就创建 （**一般用它**）

#### ApplicationContext的实现类

- ClassPathXmlApplicationContext：
它是从类的根路径下加载配置文件 推荐
- FileSystemXmlApplicationContext：
它是从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置。
- AnnotationConfigApplicationContext:
当我们使用注解配置容器对象时，需要使用此类来创建spring 容器。它用来读取注解。

#### Bean标签

**属性：**

1. 作用
   1. 用于配置对象让Spring创建
   2. 默认调用对象的无参构造器构造
2. 属性
   1. id:给对象指定唯一标识符
   2. class：指定对象的全限定类名
   3. scope：指定对象的作用范围
      1. singleton :默认值，单例的. 
      2. prototype :多例的.  
      3. request :WEB项目中,Spring创建一个Bean的对象,将对象存入到request域中. 
      4. session :WEB项目中,Spring创建一个Bean的对象,将对象存入到session域中. 
      5. global session :WEB项目中,应用在Portlet环境.如果没有Portlet环境那么globalSession相当于session.
   4. init-method：指定类中的初始化方法名称 
   5. destroy-method：指定类中销毁方法名称

#### 实例化Bean

1. 使用默认无参构造函数

```xml
<bean id="Xxx" class="xxx.xxx.xxx.Xxx"/>
```

2. 使用静态工厂

```java
public class StaticFactory { 
    public static Xxx name(){ 
        return new Xxx(); 
    } 
}
```

```xml
<!-- 此种方式是: 使用StaticFactory类中的静态方法name创建对象，并存入spring容器 
id属性：指定bean的id，用于从容器中获取 
class属性：指定静态工厂的全限定类名 
factory-method属性：指定生产对象的静态方法 -->
<bean id="xxx" class="xxx.xxx.xxx.StaticFactory" factory-method="name"></bean>
```

3. 实例工厂

```java
public class InstanceFactory { 
    public Xxx name(){ 
        return new Xxx(); 
    } 
}
```

```xml
<!-- 此种方式是： 先把工厂的创建交给spring来管理。 然后在使用工厂的bean来调用里面的方法 
factory-bean属性：用于指定实例工厂bean的id。 
factory-method属性：用于指定实例工厂中创建对象的方法。 -->
<bean id="instancFactory" class="xxx.xxx.xxx.InstanceFactory"></bean><!--工厂类--> 
<bean id="xxx" factory-bean="instancFactory" factory-method="name"></bean> <!--工厂中的方法-->
```



### 依赖注入

理解控制反转和依赖注入：https://zhuanlan.zhihu.com/p/33492169

#### 构造函数注入

使用类中的构造函数，给成员变量赋值

 ```xml
<!-- 
constructor-arg :
	属性：
		index:指定参数在构造函数参数列表的索引位置，从0开始
		type:指定参数在构造函数中的数据类型
		name:指定参数在构造函数中的名称

		value:它能赋的值是基本数据类型和String类型
		ref:它能赋的值是其他bean类型，也就是说，必须得是在配置文件中配置过的bean

-->
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"> 
    <constructor-arg name="name" value="张三"></constructor-arg>
    <constructor-arg name="age" value="18"></constructor-arg> 
    <constructor-arg name="birthday" ref="now"></constructor-arg> 
</bean> 
<bean id="now" class="java.util.Date"></bean>
 ```

#### set方法注入

```xml
<!-- 通过配置文件给bean中的属性传值：使用set方法的方式 (常用)
property
	属性： 
		name：找的是类中set方法后面的部分 
		ref：给属性赋值是其他bean类型的 
		value：给属性赋值是基本数据类型和string类型的 -->
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"> 
    <property name="name" value="test"></property> 
    <property name="age" value="21"></property> 
    <property name="birthday" ref="now"></property> 
</bean> 
<bean id="now" class="java.util.Date"></bean>
```

#### 注入集合属性

```xml
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
<!-- 在注入集合数据时，只要结构相同，标签可以互换 -->
<!-- 给数组注入数据 -->
<property name="myStrs">
    <set>
        <value>AAA</value>
        <value>BBB</value>
        <value>CCC</value>
    </set>
</property>
<!-- 注入 list 集合数据 -->
<property name="myList">
    <array>
        <value>AAA</value>
        <value>BBB</value>
        <value>CCC</value>
    </array>
</property>
<!-- 注入 set 集合数据 -->
<property name="mySet">
    <list>
        <value>AAA</value>
        <value>BBB</value>
        <value>CCC</value>
    </list>
</property>
<!-- 注入 Map 数据 -->
<property name="myMap">
    <props>
        <prop key="testA">aaa</prop>
        <prop key="testB">bbb</prop>
    </props>
</property>
<!-- 注入 properties 数据 -->
<property name="myProps">
    <map>
        <entry key="testA" value="aaa"></entry>
        <entry key="testB">
        	<value>bbb</value>
        </entry>
    </map>
</property>
</bean>
```

## AOP

## 注解

 ### 创建对象

@Component

​	用于把当前类对象存入Spring容器种，相当于配置文件里的\<bean>\</bean>

这个注解有一个属性value用于指定bean的id，默认值为当前类名，首字母小写

@Controller @Service @Repository  这三个注解的作用和属性与@Component相同，只是提供了更加明确的语义化  

### 注入数据

相当于\<property name="" ref="">  

**@AutoWired** 自动按照类型注入  

使用注解注入属性时， set 方法可以省略

当有多个类型匹配时，使用要注入的对象变量名称作为 bean 的 id，在 spring 容器查找    

**@Qualifier**  在自动按照类型注入的基础之上，再按照 Bean 的 id 注入。 

给字段注入时不能独立使用，必须和@Autowire 一起使用;给方法参数注入时，可以独立使用。  

属性：value，bean 的id

**@Resource**  直接按照 Bean 的 id 注入 

属性：value，bean 的id

**@Value**  注入基本数据类型和 String 类型数据的  

value：用于指定值  