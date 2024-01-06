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
title: 8-REST API
date:  2023-11-11 18:11
modified:  2024-01-06 09:01
---

# 1. 不同开发模式

## 1.1. 前后端不分离

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F02%2F5ec4e681073f4e19bdfa388580e8f2ac_20231102185803.png)

## 1.2. 前后端分离

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F02%2F458ecc31fe35e9d03f9062f9439a2ff3_20231102185749.png)

# 2. 使用Spring MVC的控制器创建RESTful端点

## 2.1. Rest原则👍

- Representational State Transfer，表述性状态转移
	- 表述性（Representational）：REST 资源实际上可以用各种形式来进行表述，包括json、xml、html、pdf、excel
		- 服务端的资源在客户端的表现形式
	- 状态 (State): 资源在特定时间的条件或属性。当使用 REST 的时候，我们更关注资源的状态而不是对资源采取的行为；
	- 转移（Transfer）：REST 涉及到转移资源数据，它以某种表述性形式从一个应用转移到另一个应用;
		- 服务端--客户端, 从一端到另一端
- **资源（Resources**）：就是网络上的一个实体
	- 标识：URI
- HTTP协议的四个操作方式的动词：GET、POST、PUT、DELETE
	- **CRUD**：Create、Read、Update、Delete
	- GET: Read
	- POST: Create
	- PUT: Update
	- DELETE: Delete

更简洁地讲，REST 就是**将资源的状态以最适合客户端或服务端的形式从服务器端转移到客户端（或者反过来）**。  
如果一个架构符合REST原则，就称它为RESTful架构. 请求都是针对资源的操作.

## 2.2. RESTful 控制器实现👍

- RESR API以面向数据的格式返回, JSON或XML
	- JSON更直观，结构清晰
- **@RestController, @ResponseBody**: 
	- 返回JSON格式串, 而不是逻辑视图名，或返回ResponseEntity对象
	- 第三方包完成了java对象到JSON格式串的转换过程
	- <font color="#ff0000">区别</font>：在控制器**方法**上方加ResponseBody会返回JSON串；RestController加在**类**上，相当于每个方法上都加ResponseBody
- @RequestMapping的**produces**属性
	- 数组
	- 用于请求映射，指定需要处理的数据格式,
	- e.g. `produces={"application/json", "application/xml"}`
- **@RequestBody**：用于方法参数，把从客户端来的JSON串转换成java对象。

各种Mapping的注解不变，可以继续用。

## 2.3. 请求头与请求体

- 请求头：请求头由 key/value 对组成，每行为一对，key 和 value 之间通过冒号(:)分割。请求头的作用主要用于通 知服务端有关于客户端的请求信息。
	- User-Agent：生成请求的浏览器类型
	- <font color="#ff0000">Accept：客户端可识别的响应内容类型列表；星号* 用于按范围将类型分组。*/*表示可接受全部类型，type/*表示可接受 type 类型的所有子类型。</font>
	- Accept-Language: 客户端可接受的自然语言
	- Accept-Encoding: 客户端可接受的编码压缩格式
	- Accept-Charset： 可接受的字符集
	- Host: 请求的主机名，允许多个域名绑定同一 IP 地址
	- connection：连接方式（close 或 keepalive）
	- Cookie: 存储在客户端的扩展字段
	- Content-Type:标识请求内容的类型
	- Content-Length:标识请求内容的长度
- 请求体：请求体主要用于 POST 请求，与 POST 请求方法配套的请求头一般有 Content-Type和 Content-Length

### 2.3.1. Accept取值

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
- **application/json： JSON数据格式**
- application/pdf：pdf格式 
- application/msword： Word文档格式
- application/octet-stream： 二进制流数据（如常见的文件下载）
- application/x-www-form-urlencoded： < form encType=””>中默认的encType，form表单数据被编码为key/value格式发送到服务器（表单默认的提交数据的格式）

## 2.4. 响应头与响应体👍

- 状态行：由 HTTP 协议版本、状态码、状态码描述三部分构成，它们之间由空格隔开。
- <font color="#ff0000">状态码</font>：由 3 位数字组成，**第一位标识响应的类型**，常用的**5大类状态码**如下：
	- 1xx：表示服务器已接收了客户端的请求，客户端可以继续发送请求
	- 2xx：表示服务器已成功接收到请求并进行处理
		- 201：Created
	- 3xx：表示服务器要求客户端重定向
	- 4xx：表示客户端的请求有非法内容
	- 5xx：标识服务器未能正常处理客户端的请求而出现意外错误
- 响应头
	- Location：服务器返回给客户端，用于重定向到新的位置
	- Server： 包含服务器用来处理请求的软件信息及版本信息Vary：标识不可缓存的请求头列表
	- Connection: 连接方式， close 是告诉服务端，断开连接，不用等待后续的请求了。 keep-alive 则是告诉服务端，在完成本次请求的响应后，保持连接
	- Keep-Alive: 300，期望服务端保持连接多长时间（秒）
- 响应内容：服务端返回给请求端的文本信息。

## 2.5. @CrossOrigin注解

CORS ，Cross Origin Resource Sharing

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F02%2Fd8874d4407769d49e74b327b797681ef_20231102201911.png)

前端返回页面即浏览器访问consumer, 但又需要访问后端. 两者IP地址不同, 浏览器默认禁止跨域访问. 在Controller类上`@CrossOrigin(<>)`注明运行跨域访问的地址.

请求头内HOST字段会有主机域名和端口号

## 2.6. 消息转换器

- 使用注解**方法级@ResponseBody**或**类级@RestController**
	- 作用：指定使用消息转换器 
- 没有model和视图，控制器产生数据，然后消息转换器转换数据之后的资源表述。
- Spring会自动注入一些HttpMethodConverter, 如jackson Json processor、JAXB库
- 请求传入，**@RequestBody**以及HttpMethodConverter

应用：jackson Json processor帮助spring转换java object和json

## 2.7. @GetMapping

- @PathVariable路径参数据
- 返回ResponseEntity  
如果未查询到元素，返回状态码200，body返回null，如果不使用Optional类型，则返回状态码500

## 2.8. @PostMapping

- consumes属性
- @RequestBody
- @ResponseStatus，指定**返回状态码**

## 2.9. @PutMapping和@PatchMapping

- @PutMapping，putOrder方法实现，”将数据放到这个URL上“
	- 完全覆盖
- @PatchMapping，patchOrder方法实现
	- 局部更新

协议规定了两者更新的范围不一样， 实际开发很少用Patch

## 2.10. @DeleteMapping

@DeleteMapping，deleteOrder方法实现，HttpStatus.NO_CONTENT(204)，body不需要返回数据

## 2.11. 参数类型

1. 表单参数 form
	- 用户名、密码
	- 不需要任何注解，MVC框架自动转换
2. 请求体参数
	- 请求体转成java对象
3. 查询参数
	- `RequestParam`
4. 路径参数
	- `@PathVariable`

## 2.12. Rest API接口设计👍

**要求：**

1. 使用**标准HTTP动词**：GET、PUT、POST、DELETE，映射到CRUD
2. 使用URL来传达意图
	- 例：请求一批资源复数，单个资源单数
	- 推荐用名词
3. 请求和响应使用JSON
4. 使用HTTP状态码来传达效果
	- `Create`: 201
	- `No content`: 204

# 3. 将Spring Data存储库暴露为REST端点

- 添加spring data的rest子依赖：`spring-boot-starter-data-rest`

## 3.1. Spring HEATEOAS项目

- 超媒体作为应用状态引擎（Hypermedia AsThe Engine Of Application State,HEATEOAS）
- 消费这个API的客户端可以使用这些超链接作为指南，以便于导航API并执行后续的请求
- 也会生成POST和PUT请求

## 3.2. 设置API基础路径

```yaml
spring:
  data
    rest:
      base-path: /data-api
```

## 3.3. 调整关系名和路径

```java
@Data
@Entity
@RestResource(rel="tacos", path="tacos") 
public class Taco {}
```

甚至可以在URL种实现分页和排序：  
http://tacocloud:8080/data-api/tacos?size=15&page=0&sort=createdAt,desc

# 4. 测试和保护端点

## 4.1. RestTemplate

spring会自动注入，不需要自己创建。在java中调用RESTful API。

getForObject：只获取body  
getForEntity：获取完整responseEntity，可以获取headers

## 4.2. 使用Feign调用REST API

用得更多，实现调用远程API就像调用本地对象。

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-feign</artifactId>
</dependency>
```