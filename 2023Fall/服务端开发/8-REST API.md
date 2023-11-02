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

# RESTful 控制器实现

- RESR API以面向数据的格式返回, JSON或XML
- @RestController, @ResponseBody: 返回JSON格式串, 而不是逻辑视图名
	- 或返回ResponseEntity对象，TacoController
	- 第三方包完成了java对象到JSON格式串的转换过程
- @RequestMapping的produces属性
	- 数组
	- 指定处理的数据格式, `produces={"application/json", "application/xml"}`

各种Mapping不变

# Accept取值

- text/html ： HTML格式
- text/plain ：纯文本格式 
- text/xml ： XML格式
- image/gif ：gif图片格式 
- image/jpeg ：jpg图片格式
- image/png：png图片格式
- video/mpeg：视频
- vedio/quicktime：视频
- application/xhtml+xml ：XHTML格式
- application/xml： XML数据格式
- application/atom+xml ：Atom XML聚合格式 
- application/json： JSON数据格式
- application/pdf：pdf格式 
- application/msword： Word文档格式
- application/octet-stream： 二进制流数据（如常见的文件下载）
- application/x-www-form-urlencoded： < form encType=””>中默认的encType，form表单数据被编码为key/value格式发送到服务器（表单默认的提交数据的格式）

# CrossOrigin

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F02%2Fd8874d4407769d49e74b327b797681ef_20231102201911.png)

前端返回页面即浏览器访问consumer, 但又需要访问后端. 两者IP地址不同, 浏览器默认禁止跨域访问. 在Controller类上`@CrossOrigin(<>)`注明运行跨域访问的地址