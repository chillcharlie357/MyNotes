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
title: 9-OAuth2
date:  2023-11-13 18:11
modified:  2023-12-31 16:12
---

# 1. 创建授权服务器

## 1.1. 增加权限控制

POST：http://tacocloud:8080/api/ingredients  
DELETE ：http://tacocloud:8080 /api/ingredients/*

- 两种方式
	- .antMatchers(HttpMethod.POST, "/api/ingredients").hasAuthority("ROLE_USER")
	- @PreAuthorize("hasAuthority('ROLE_USER')")

## 1.2. OAth2👍

之前讲了Spring Security的单体应用权限控制

- OAth2：**分布式环境**中微服务权限控制，需要把认证和授权集中在一个地方，不需要每个微服务单独做认证授权。
	- 是一个与变成语言无关的规范
	- restful api权限控制

## 1.3. 授仅码授权（authorization code grant）模式👍

### 1.3.1. 流程图👍

- Client application：客户端（第三方应用程序），消费API提供的资源
- Authorization server：授权服务器
- Resource server：提供API资源

![msedge_CL3vHC98yN.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F13%2F19-20-30-a83b91bcb885875b08c329f60f5ac115-msedge_CL3vHC98yN-06bb6b.png)

- 授权服务器会用**私钥**给token签名，资源服务器用**公钥**验证token是否合法
- token不变时公钥也不变，只有第一次才需要向授权服务器索取公钥

### 1.3.2. 过程👍

其中使用**授权码授权模式**

1. 用户使用第三方的应用程序，也就是客户端应用程序
2. 客户端发现用户未登录，把用户请求**重定向**到授权服务器
	- 授权服务器会维护合法的重定向地址，用于校验
3. 授权服务器向用户索取用户名密码
4. 用户名密码匹配，则授权服务器请求用户授权
5. **授权服务器给客户端程序返回code**，重定向回到应用程序
	- code需要经过浏览器
6. **客户端应用程序用code向授权服务器索取token**
	- 用code交换token
	- token不过浏览器，在应用程序服务端和授权服务器之间处理
7. 客户端在请求头带上token调用资源服务器的API
8. 资源服务器验证token，返回结果
	- 第一次，资源服务器向授权服务器索取公钥，验证Token合法性
	- Token过期时才会重新索取公钥
9. 客户端程序把结果返回给用户

密码没有在浏览器来回传送，但是如果没有密码即使拿到code也没用  
用code向授权服务器换取token才需要密码

## 1.4. 其他授权模式

1. 隐式授权（implicit grant）：直接返回访问令牌Token，而不是授仅码
2. 用户凭证（或密码）授权（user credentials (password) grant）：用户凭证直接换取访问令牌，不经过浏览器登录
3. 客户端凭证授权（client credentials grant）：客户端交换自己的凭证以获取访问令牌

## 1.5. 开发授权服务器

### 1.5.1. 授权服务器config

1. 注册第三方应用程序（客户端）
2. 指定资源服务器地址
3. jwkSource

### 1.5.2. JWK

JSON web key，RSA密钥对（公䄴、私䄴），用于对令牌签名，令牌会用私钥签名，资源服务器会通过从授权 服务器获取到的公钥验证请求中收到的令牌是否有效

运行授权服务器

### 1.5.3. 运行授权服务器

1. 获得授权码（浏览器访问）:http://authserver:9000/oauth2/authorize?response_type=code&client_id=taco-admin-client&scope=writeIngredients+deleteIngredients&redirect_uri=http://clientadmin:9090/login/oauth2
2. 获得token（postman访问） POST ： http://authserver:9000/oauth2/token
3. 刷新token（postman访问）： http://authserver:9000/oauth2/token

# 2. 创建资源服务器

## 2.1. 步骤

1. 添加依赖

2. 在过滤器中针对被保护的API添加权限控制
	- 使用`SCOPE_`前缀
	- 开启API调用前的过滤器

3. 指定授权服务器的地址
	- 为了获取公钥

<font color="#ff0000">使用`SCOPE_`前缀</font>:

```
.antMatchers(HttpMethod.POST, "/api/ingredients").hasAuthority("SCOPE_writeIngredients")
.antMatchers(HttpMethod.DELETE, "/api/ingredients/*").hasAuthority("SCOPE_deleteIngredients")
```

开启API调用前的过滤器:

```
and()
	.oauth2ResourceServer(oauth2 -> oauth2.jwt())
```

## 2.2. 配置资源服务器从何处获取公钥

jwt属性配置：

```
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          jwk-set-uri: http://tacocloud:9000/oauth2/jwks
```

## 2.3. 从POSTMAN访问被保护资源

- 带着token（Authorization属性）访问POST：http://tacocloud:8080/api/ingredients
- 带着token（Authorization属性）访问DELETE：http://tacocloud:8080/api/ingredients/*

# 3. 开发客户端