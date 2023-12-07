---
aliases: 
tags: 
categories:
sticky:
thumbnail:
cover: 
excerpt: false
mathjax: true
comment: true
title: 13-WebFlux
date:  Thursday,December 7th 2023
modified:  Thursday,December 7th 2023
---

# 异步Web框架的事件轮询机制

- 用较少的线程处理更多请求，减少线程管理的开销
	- 很多请求的线程并不是在运行，而是在等待
	- 事件驱动

反应式编程适合**请求多**且**大部分在等待**的情况

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F07%2F18-52-05-3d823dafa4673e8da18904eae9cbd7f0-20231207185204-00c0c0.png)

# Spring MVC与Spring WebFlux

- 不同
	- MVC：依赖多线程处理
	- WebFlux：在事件轮询中处理请求
		- 可以使用纯粹的函数式编程实现Controller
- 共性
	- WebFlux也可以使用Controller、RequestMapping等注解
		- WebFlux的参数和返回值可能是流

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F07%2F19-00-35-8e48541635a29f83c6e89b87c0f1ee0d-20231207190034-169096.png)

依赖：spring-boot-starter-webflux

# 实现

## repository

继承`ReactiveCRUDrepository`

