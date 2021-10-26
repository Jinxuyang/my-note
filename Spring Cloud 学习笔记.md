---
title: Spring Cloud学习笔记
date: 2020-11-09 00:04:09
tags:
	- spring
	- spring cloud
	- 学习笔记
categories: 
	- spring
---

[TOC]

# Spring Cloud 学习笔记

## 版本选择

通过访问`https://start.spring.io/actuator/info`

<!--more-->

```json
{
	"git": {
		"branch": "82af3869647d62a1e520a076908c14eee4715d8d",
		"commit": {
			"id": "82af386",
			"time": "2020-11-02T15:56:02Z"
		}
	},
	"build": {
		"version": "0.0.1-SNAPSHOT",
		"artifact": "start-site",
		"versions": {
			"spring-boot": "2.3.5.RELEASE",
			"initializr": "0.10.0-SNAPSHOT"
		},
		"name": "start.spring.io website",
		"time": "2020-11-02T16:15:14.702Z",
		"group": "io.spring.start"
	},
	"bom-ranges": {
		"azure": {
			"2.0.10": "Spring Boot >=2.0.0.RELEASE and <2.1.0.RELEASE",
			"2.1.10": "Spring Boot >=2.1.0.RELEASE and <2.2.0.M1",
			"2.2.4": "Spring Boot >=2.2.0.M1 and <2.3.0.M1",
			"2.3.5": "Spring Boot >=2.3.0.M1"
		},
		"codecentric-spring-boot-admin": {
			"2.0.6": "Spring Boot >=2.0.0.M1 and <2.1.0.M1",
			"2.1.6": "Spring Boot >=2.1.0.M1 and <2.2.0.M1",
			"2.2.4": "Spring Boot >=2.2.0.M1 and <2.3.0.M1",
			"2.3.0": "Spring Boot >=2.3.0.M1 and <2.4.0-M1"
		},
		"solace-spring-boot": {
			"1.0.0": "Spring Boot >=2.2.0.RELEASE and <2.3.0.M1",
			"1.1.0": "Spring Boot >=2.3.0.M1"
		},
		"solace-spring-cloud": {
			"1.0.0": "Spring Boot >=2.2.0.RELEASE and <2.3.0.M1",
			"1.1.1": "Spring Boot >=2.3.0.M1"
		},
		"spring-cloud": {
			"Finchley.M2": "Spring Boot >=2.0.0.M3 and <2.0.0.M5",
			"Finchley.M3": "Spring Boot >=2.0.0.M5 and <=2.0.0.M5",
			"Finchley.M4": "Spring Boot >=2.0.0.M6 and <=2.0.0.M6",
			"Finchley.M5": "Spring Boot >=2.0.0.M7 and <=2.0.0.M7",
			"Finchley.M6": "Spring Boot >=2.0.0.RC1 and <=2.0.0.RC1",
			"Finchley.M7": "Spring Boot >=2.0.0.RC2 and <=2.0.0.RC2",
			"Finchley.M9": "Spring Boot >=2.0.0.RELEASE and <=2.0.0.RELEASE",
			"Finchley.RC1": "Spring Boot >=2.0.1.RELEASE and <2.0.2.RELEASE",
			"Finchley.RC2": "Spring Boot >=2.0.2.RELEASE and <2.0.3.RELEASE",
			"Finchley.SR4": "Spring Boot >=2.0.3.RELEASE and <2.0.999.BUILD-SNAPSHOT",
			"Finchley.BUILD-SNAPSHOT": "Spring Boot >=2.0.999.BUILD-SNAPSHOT and <2.1.0.M3",
			"Greenwich.M1": "Spring Boot >=2.1.0.M3 and <2.1.0.RELEASE",
			"Greenwich.SR6": "Spring Boot >=2.1.0.RELEASE and <2.1.999.BUILD-SNAPSHOT",
			"Greenwich.BUILD-SNAPSHOT": "Spring Boot >=2.1.999.BUILD-SNAPSHOT and <2.2.0.M4",
			"Hoxton.SR8": "Spring Boot >=2.2.0.M4 and <2.3.6.BUILD-SNAPSHOT",
			"Hoxton.BUILD-SNAPSHOT": "Spring Boot >=2.3.6.BUILD-SNAPSHOT and <2.4.0.M1",
			"2020.0.0-M3": "Spring Boot >=2.4.0.M1 and <=2.4.0.M1",
			"2020.0.0-M4": "Spring Boot >=2.4.0.M2 and <=2.4.0-M3",
			"2020.0.0-SNAPSHOT": "Spring Boot >=2.4.0-M4"
		},
		"spring-cloud-alibaba": {
			"2.2.1.RELEASE": "Spring Boot >=2.2.0.RELEASE and <2.3.0.M1"
		},
		"spring-cloud-services": {
			"2.0.3.RELEASE": "Spring Boot >=2.0.0.RELEASE and <2.1.0.RELEASE",
			"2.1.8.RELEASE": "Spring Boot >=2.1.0.RELEASE and <2.2.0.RELEASE",
			"2.2.6.RELEASE": "Spring Boot >=2.2.0.RELEASE and <2.3.0.RELEASE",
			"2.3.0.RELEASE": "Spring Boot >=2.3.0.RELEASE and <2.4.0-M1"
		},
		"spring-statemachine": {
			"2.0.0.M4": "Spring Boot >=2.0.0.RC1 and <=2.0.0.RC1",
			"2.0.0.M5": "Spring Boot >=2.0.0.RC2 and <=2.0.0.RC2",
			"2.0.1.RELEASE": "Spring Boot >=2.0.0.RELEASE"
		},
		"vaadin": {
			"10.0.17": "Spring Boot >=2.0.0.M1 and <2.1.0.M1",
			"14.4.2": "Spring Boot >=2.1.0.M1 and <2.4.0-M1"
		},
		"wavefront": {
			"2.0.2": "Spring Boot >=2.1.0.RELEASE and <2.4.0-M1",
			"2.1.0-SNAPSHOT": "Spring Boot >=2.4.0-M1"
		}
	},
	"dependency-ranges": {
		"okta": {
			"1.2.1": "Spring Boot >=2.1.2.RELEASE and <2.2.0.M1",
			"1.4.0": "Spring Boot >=2.2.0.M1 and <2.4.0-M1"
		},
		"mybatis": {
			"2.0.1": "Spring Boot >=2.0.0.RELEASE and <2.1.0.RELEASE",
			"2.1.3": "Spring Boot >=2.1.0.RELEASE and <2.4.0-M1"
		},
		"geode": {
			"1.2.10.RELEASE": "Spring Boot >=2.2.0.M5 and <2.3.0.M1",
			"1.3.4.RELEASE": "Spring Boot >=2.3.0.M1 and <2.4.0-M1",
			"1.4.0-M4": "Spring Boot >=2.4.0-M1"
		},
		"camel": {
			"2.22.4": "Spring Boot >=2.0.0.M1 and <2.1.0.M1",
			"2.25.2": "Spring Boot >=2.1.0.M1 and <2.2.0.M1",
			"3.3.0": "Spring Boot >=2.2.0.M1 and <2.3.0.M1",
			"3.5.0": "Spring Boot >=2.3.0.M1 and <2.4.0-M1"
		},
		"open-service-broker": {
			"2.1.3.RELEASE": "Spring Boot >=2.0.0.RELEASE and <2.1.0.M1",
			"3.0.4.RELEASE": "Spring Boot >=2.1.0.M1 and <2.2.0.M1",
			"3.1.1.RELEASE": "Spring Boot >=2.2.0.M1 and <2.4.0-M1"
		}
	}
}
```

选择Spring boot版本和Spring cloud版本

## 引入spring cloud alibaba

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2.2.3.RELEASE</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

然后在 `dependencies` 中添加自己所需使用的依赖即可使用。

### 版本管理规范

项目的版本号格式为 x.x.x 的形式，其中 x 的数值类型为数字，从 0 开始取值，且不限于 0~9 这个范围。项目处于孵化器阶段时，第一位版本号固定使用 0，即版本号为 0.x.x 的格式。

由于 Spring Boot 1 和 Spring Boot 2 在 Actuator 模块的接口和注解有很大的变更，且 spring-cloud-commons 从 1.x.x 版本升级到 2.0.0 版本也有较大的变更，因此我们采取跟 SpringBoot 版本号一致的版本:

- 1.5.x 版本适用于 Spring Boot 1.5.x
- 2.0.x 版本适用于 Spring Boot 2.0.x
- 2.1.x 版本适用于 Spring Boot 2.1.x
- 2.2.x 版本适用于 Spring Boot 2.2.x

## 使用nacos

### 下载安装

下载地址https://github.com/alibaba/nacos/releases

windows上运行下载.zip文件，运行startup.cmd

默认端口为8848

### 注册

配置文件

```yml
spring:
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
  application:
    name: application name
```

## 使用OpenFeign

### 引入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

### 写接口

```java
@FeignClient("要调用的服务名")
public interface CouponFeignService {
    @RequestMapping("路径")
    public R list(@RequestParam Map<String, Object> params);// 与被调用的方法相同
}

```

### 开启远程调用功能

在spring启动类中添加

```java
@EnableFeignClients(basePackages = "接口所在包")
```

## Nacos配置中心

### 引入依赖

```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

### 配置文件

新建bootstrap.yml

```yml
spring:    
    cloud:
        nacos:
          config:
            server-addr: localhost:8848
    application:
        name: gulimall-coupon
```

### 配置中心添加

应用名.后缀

### 动态获取配置

```java
@RefreshScope
```

配置中心和本地配置冲突时，优先使用配置中心

### 其他

#### 命名空间：配置隔离

例如：

1. 开发环境，测试环境，生产环境
2. 微服务



修改默认命名空间

```yml
spring:
  cloud:
    nacos:
      config:
        server-addr: localhost:8848
        namespace: 5c990c25-30e9-4490-8e52-aaa45d8bbeb9 命名空间id
```

![img](D:\Personal\Blog\source\_posts\img\84@_NEV9[]LIK_WB9~WV{VI.png)

#### 配置集

#### 配置分组

## 网关Spring Cloud Gateway

### 引入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```

### 出现the 'Access-Control-Allow-Origin' header contains multiple values

![1](D:\Personal\Blog\source\_posts\img\1.png)

这是spring的一个问题

可以通过修改配置文件解决

```yml
spring:
  cloud:
    gateway:
      default-filters:
        - DedupeResponseHeader=Access-Control-Allow-Origin Access-Control-Allow-Credentials, RETAIN_UNIQUE
```

