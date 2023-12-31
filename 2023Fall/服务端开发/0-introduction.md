---
aliases: 
tags:
  - 2023_Fall_服务端开发
  - 课程
categories: 2023_Fall_服务端开发
sticky: 
thumbnail: 
cover: 
excerpt: false
mathjax: true
comment: true
title: 0-introduction
date: 2023-10-10 18:22
modified: 2023-12-29 10:15
---

# 1. Spring👍

- Spring
	- Spring的核心是提供一个容器（container）
	- 管理Bean的生命周期，组装Bean形成一个可用的系统
	- Application Context上下文运行环境
- Bean
	- 一个object  

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F09%2F07%2F27a33f228b702e3362eab03541d8c21e_20230907191141.png)

- 核心技术
	- 依赖注入
	- AOP面向切面的编程

- 持久层
	- 简化，只需要关注业务
	- JDBC、事务管理、ORM工具整合

# 2. Spring衍生

- Spring：基本框架
- Spring Boot：自动配置，简化Spring开发
	- 开发单个微服务（可执行程序）
- Spring Cloud：解决分布式系统中特有的问题

## 2.1. spring boot

1. 可以创建独立的Spring应用程序，并且基于其Maven或Gradle插件，可以创建可执行的JARs和WARs
2. 内嵌Tomcat或Jetty等**Servlet容器**（Web容器，有很多可运行的web组件）
3. 提供自动配置的“**starter**”项目对象模型（POMS）以简化Maven配置
4. 尽可能自动配置**Spring容器**
5. 提供准备好的特性，如指标、健康检查和外部化配置
6. 绝对没有代码生成，不需要XML配置

war包小，不能独立运行  
Jar包大，内嵌tomcat容器，本身可以独立运行

## 2.2. 常用依赖

devtools：开发时候用于调试  
web：MVC开发框架  
thymeleaf：动态组装html页面

# 3. Spring Web开发框架的分层👍

- 在服务端一侧的划分：

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F09%2F07%2Faa30cd55d3482bfd4228c3f361bba520_20230907204339.png)

# 4. 使用JUnit写测试用例

- `@Test`注解
- `void`返回值，`public`可选
- 无参数

-方法命名随意

## 4.1. 断言

测试代码中对预期结果的断定

# 5. Spring模块组成

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F09%2F14%2F4df50c54d812c48ff73dfeac3b4008cc_20230914185434.png)
