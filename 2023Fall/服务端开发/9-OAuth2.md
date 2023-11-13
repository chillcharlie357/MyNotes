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
title: 9-OAuth2
date:  Monday,November 13th 2023
modified:  Monday,November 13th 2023
---

之前讲了Spring Security的单体应用权限控制

- OAth2：分布式环境中微服务权限控制，需要把认证和授权集中在一个地方
	- restful api权限控制

# 创建授权服务器

## 增加权限控制

POST：http://tacocloud:8080/api/ingredients  
DELETE ：http://tacocloud:8080 /api/ingredients/*

- 两种方式
	- .antMatchers(HttpMethod.POST, "/api/ingredients").hasAuthority("ROLE_USER")
	- @PreAuthorize("hasAuthority('ROLE_USER')")

## OAth2👍

是一个与变成语言无关的规范

## 授仅码授权（authorization code grant）模式👍

用code交换token

- Client application：客户端（第三方应用程序），消费API提供的资源
- Authorization server：授权服务器
- Resource server：提供API资源

![msedge_CL3vHC98yN.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F13%2F19-20-30-a83b91bcb885875b08c329f60f5ac115-msedge_CL3vHC98yN-06bb6b.png)

授权服务器会用私钥给token签名，资源服务器用公钥验证token是否合法

### 过程

1. 用户使用第三方的应用程序，也就是客户端应用程序
2. 客户端发现用户未登录，把用户重请求定向到授权服务器
3. 授权服务器向用户索取用户名密码
4. 用户名密码匹配，则授权服务器请求用户授权
5. **授权服务器给客户端程序返回code**
6. **客户端应用程序用code向授权服务器索取token**
7. 客户端在请求头带上token调用资源服务器的API
8. 资源服务器验证token，返回结果
9. 客户端程序把结果返回给用户

## 其他授权模式

1. 隐式授权（implicit grant），直接返回访问令牌，而不是授仅码
2. 用户凭证（或密码）授权（user credentials (password) grant）：用户凭证直接换取访问令牌，不经过浏览器登录
3. 客户端凭证授权（client credentials grant）：客户端交换自己的凭证以获取访问令牌

## 开发授权服务器

### 授权服务器config

1. 注册第三方应用程序（客户端）
2. 指定资源服务器地址
3. jwkSource

### JWK

JSON web key，RSA密钥对（公䄴、私䄴），用于对令牌签名，令牌会用私钥签名，资源服务器会通过从授权 服务器获取到的公钥验证请求中收到的令牌是否有效

运行授权服务器

### 运行授权服务器

1. 获得授权码（浏览器访问）:http://authserver:9000/oauth2/authorize?response_type=code&client_id=taco-admin-client&scope=writeIngredients+deleteIngredients&redirect_uri=http://clientadmin:9090/login/oauth2
2. 获得token（postman访问） POST ： http://authserver:9000/oauth2/token
3. 刷新token（postman访问）： http://authserver:9000/oauth2/token


# 创建资源服务器

1. 添加依赖
2. 在过滤器中针对被保护的API添加权限控制
	- 使用`SCOPE_`前缀
3. 指定授权服务器的地址
	- 为了获取公钥