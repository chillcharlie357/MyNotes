
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

- 授仅码授权（authorization code grant）模式
	- Client application：客户端（第三方应用程序），消费API提供的资源
	- Authorization server：授权服务器
	- Resource server：提供API资源

![msedge_CL3vHC98yN.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F13%2F19-20-30-a83b91bcb885875b08c329f60f5ac115-msedge_CL3vHC98yN-06bb6b.png)

授权服务器会用私钥给token签名，资源服务器用公钥验证token是否合法

用户。使用第三方的应用程序，也就是客户端应用程序
# 创建资源服务器