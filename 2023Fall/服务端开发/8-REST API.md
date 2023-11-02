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
title: 8-REST API
date:  Thursday,November 2nd 2023
modified:  Thursday,November 2nd 2023
---

# 不同开发模式

## 前后端不分离

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F02%2F5ec4e681073f4e19bdfa388580e8f2ac_20231102185803.png)

## 前后端分离

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F02%2F458ecc31fe35e9d03f9062f9439a2ff3_20231102185749.png)

# Rest

- Representational State Transfer，表现层状态转移
	- 表现层（Representation）：json、xml、html、pdf、excel
		- 服务端的资源在客户端的表现形式
	- 状态转移（State Transfer）：服务端--客户端, 从一端到另一端
- 资源（Resources），就是网络上的一个实体，标识：URI
- HTTP协议的四个操作方式的动词：GET、POST、PUT、DELETE
	- **CRUD**：Create、Read、Update、Delete
	- GET: Read
	- POST: Create
	- PUT: Update
	- DELETE: Delete

如果一个架构符合REST原则，就称它为RESTful架构. 请求都是针对资源的操作.


# Accept

