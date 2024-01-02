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
title: 13-WebFlux
date:  2023-12-07 18:12
modified:  2023-12-31 16:12
---

# 1. 异步Web框架的事件轮询机制

- 事件轮询：用较少的线程处理更多请求，减少线程管理的开销
	- 很多请求的线程并不是在运行，而是在等待
	- **事件驱动**

反应式编程适合**请求多**且**大部分在等待**的情况

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F07%2F18-52-05-3d823dafa4673e8da18904eae9cbd7f0-20231207185204-00c0c0.png)

# 2. Spring MVC与Spring WebFlux

- 不同
	- MVC：依赖多线程处理，基于servlet api实现
		- 来一个请求就开一个线程
	- WebFlux：在事件轮询中处理请求
		- 可以使用纯粹的函数式编程实现Controller
- 共性
	- WebFlux也可以使用Controller、RequestMapping等注解
		- WebFlux的参数和返回值可能是流

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F07%2F19-00-35-8e48541635a29f83c6e89b87c0f1ee0d-20231207190034-169096.png)

依赖：spring-boot-starter-webflux

# 3. 实现

## 3.1. repository

- 继承`ReactiveCRUDrepository`接口
- 成员方法返回流

# 4. controller

- 返回流
- 如果方法**没有返回值**，需要在方法内**订阅**
	- 否则不会执行，不订阅就不会驱动
	- 多层嵌套流，对外围订阅不会触发内层的流

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F07%2F19-18-18-1741f90e8a645b889a8b9171efa080c4-20231207191818-e018d8.png)

## 4.1. 使用函数式范式定义控制器

- 基本组件
	- HandlerFunction：
	- RouterFunction：路由和处理关系

WebClient：相当于RestTemplate

# 5. R2DBC

反应式关系型数据库链接  
JDBC的替代方案
