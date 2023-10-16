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
title: 6-Spring Security
date:  Monday,October 16th 2023
modified:  Monday,October 16th 2023
---
- JAAS: Java Authentication Authorization Service. JDK提供的认证授权服务

`spring-boot-starter-security`

# Cookie

- http是无状态的协议，cookie可以保存状态
	- 例：保持用户登录状态

# 开发要做什么

1. 实现接口
	- 被Spring Security调用
	- `UserDetailInterface`  
![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F10%2F16%2F0dd61f796a4a66bb935bc6ffaebd9676_20231016193806.png)

- PasswordEncoder：<font color="#ff0000">密码不能明文存储</font>，需要加密后再存到数据库里
	- 需要定义`Bean`

# CSRF

- 跨站请求伪造
- 默认开启
- 如何关闭：`.csrd().disable()`
- 如何忽略：`.ignoringAntMatchers("/admin/**")`

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F10%2F16%2F485966b40e6bb369ce1b793d15d48bf1_20231016205535.png)

- 例子：
	1. C访问A，登录，建立长期会话
	2. C每次对A请求，都会带有SessionID
	3. 在此期间C又访问了B，假设又一个表达
	4. B返回表单，但提交地址是A
	5. 由于有C的SessionID，所以A认为是合法的

- 解决：C每次提交表单A，\_csrf 字段有唯一ID，无法伪造

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F10%2F16%2F6a1a8a2f1ac8edf6f33638931d9d15b6_20231016210157.png)


# CORS：跨域资源共享

- 是由浏览器定义的协议
	- 是一种基于 [HTTP](https://developer.mozilla.org/zh-CN/docs/Glossary/HTTP) 头的机制，该机制通过允许服务器标示除了它自己以外的其他[源](https://developer.mozilla.org/zh-CN/docs/Glossary/Origin)（域、协议或端口），使得浏览器允许这些源访问加载自己的资源。跨源资源共享还通过一种机制来检查服务器是否会允许要发送的真实请求，该机制通过浏览器发起一个到服务器托管的跨源资源的“预检”请求。在预检中，浏览器发送的头中标示有 HTTP 方法和真实请求中会用到的头。
- 常用于客户端与服务端分离的场景
	- 例：A提供静态页面，B提供Rest接口


# Security权限控制

1. Java配置类权限配置
	- 针对从客户端来的http请求
2. 方法级别的权限
	- 针对业务层代码
	- 调用前控制，调用后控制
	- 例：对数据库delete操作做权限控制

