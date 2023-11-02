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
date: Thursday,November 2nd 2023
modified: Thursday,November 2nd 2023
---

# 不同开发模式

## 前后端不分离

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F02%2F5ec4e681073f4e19bdfa388580e8f2ac_20231102185803.png)

## 前后端分离

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F02%2F458ecc31fe35e9d03f9062f9439a2ff3_20231102185749.png)

# 使用Spring MVC的控制器创建RESTful端点
## Rest

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

## RESTful 控制器实现

- RESR API以面向数据的格式返回, JSON或XML
- @RestController, @ResponseBody: 返回JSON格式串, 而不是逻辑视图名
	- 或返回ResponseEntity对象，TacoController
	- 第三方包完成了java对象到JSON格式串的转换过程
- @RequestMapping的produces属性
	- 数组
	- 指定处理的数据格式, `produces={"application/json", "application/xml"}`

各种Mapping不变
## 请求头与请求体

## 响应头与响应体

### Accept取值

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

## @CrossOrigin注解

CORS ，Cross Origin Resource Sharing

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F02%2Fd8874d4407769d49e74b327b797681ef_20231102201911.png)

前端返回页面即浏览器访问consumer, 但又需要访问后端. 两者IP地址不同, 浏览器默认禁止跨域访问. 在Controller类上`@CrossOrigin(<>)`注明运行跨域访问的地址.

请求头内HOST字段会有主机域名和端口号

## 消息转换器

- 使用注解@ResponseBody或类级@RestController，作用：指定使用消息转换器 
- 没有model和视图，控制器产生数据，然后消息转换器转换数据之后的资源表述。
- Spring会自动注入一些HttpMethodConverter, 如jackson Json processor、JAXB库
- 请求传入，@RequestBody以及HttpMethodConverter

应用：jackson Json processor帮助spring转换java object和json

```java

```

## @GetMapping

- @PathVariable路径参数据
- 返回ResponseEntity  
如果未查询到元素，返回状态码200，body返回null，如果不使用Optional类型，则返回状态码500

## @PostMapping

- consumes属性
- @RequestBody
- @ResponseStatus，指定**返回状态码**

## @PutMapping和@PatchMapping

- @PutMapping，putOrder方法实现，”将数据放到这个URL上“
	- 完全覆盖
- @PatchMapping，patchOrder方法实现
	- 局部更新

协议规定了两者更新的范围不一样， 实际开发很少用Patch

## @DeleteMapping

@DeleteMapping，deleteOrder方法实现，HttpStatus.NO_CONTENT，body不需要返回数据

## 参数类型

1. 表单参数 form
	- 用户名、密码
	- 不需要任何注解，MVC框架自动转换
2. 请求体参数
	- 请求体转成java对象
3. 查询参数
	- `RequestParam`
4. 路径参数
	- `@PathVariable`

## Rest API接口设计👍

1. 使用标注HTTP动词：GET、PUT、POST、DELETE，映射到CRUD
2. 使用URL来传达意图
3. 请求和响应使用JSON
4. 使用HTTP状态码来传达效果
	- `Create`: 201
	- `No content`: 204

# 将Spring Data存储库暴露为REST端点

依赖：spring-boot-starter-data-rest

