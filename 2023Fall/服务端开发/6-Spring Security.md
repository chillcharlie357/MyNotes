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
title: 6-Spring Security
date: 2023-10-16 18:47
modified: 2023-12-29 10:15
---
- JAAS: Java Authentication Authorization Service. JDK提供的认证授权服务

`spring-boot-starter-security`

# 1. Spring Security

- 作用
	1. 针对客户web请求权限控制
	2. 针对方法级的权限控制
		- 针对业务层代码
		- 调用前控制，调用后控制
		- 例：对数据库delete操作做权限控制

添加依赖后会自动加载安全相关的bean

# 2. Cookie

- http是无状态的协议，cookie可以保存状态
	- 例：保持用户登录状态

用户登录后，服务端会返回session-id，作为cookie存在本地每次请求都带上cookie

# 3. Web请求拦截

使用filter

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F10%2F26%2F092f577948a6053f598961fe1a329feb_20231026184400.png)

# 4. 开发人员要做什么👍

<font color="#ff0000">除了框架提供的，开发人员还需要做什么?</font>

1. 实现接口
	- 被Spring Security调用
	- `UserDetailInterface`接口：给Spring框架提供用户详细信息
2. 密码加密/解密对象
3. (optional)实现登录页面
	- 有默认页面
	- `/login`, Spring已经自动实现了对应的`Controller`
4. 权限设定  
	- `SecurityFilterChain`
	- 从父类`WebSecurityConfigurerAdapter`实现`configure`方法

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F10%2F26%2F4875ccdece8ae8892f54cde6042a6fef_20231026184427.png)

- PasswordEncoder：<font color="#ff0000">密码不能明文存储</font>，需要加密后再存到数据库里
	- 需要定义`Bean`

# 5. 框架实现了什么

1. 实现用户登录控制器get post
2. 请求重定向到用户登录页面
	- eg.用户未登录时，访问URL，服务端重定向到登录页面
3. 通过Filter对用户设定的权限进行权限控制

# 6. 用户信息存储👍

- 内存用户数据库
- JDBC用户存储
- LDAP用户数据库
	- 轻量级目录数据库

# 7. 权限分类

- Authority，权限
- Role，角色$\rightarrow$权限，加**前缀**：ROLE_
	- 从角色到权限

# 8. 自定义登录页面

使用`HttpSecurity`对象配置。

- 当需要认证时转向的登录页：`.loginPage("/login")`
- 视图控制器，定义login请求对应的视图：`registry.addViewController("/login")`;
- 登录的post请求由Spring Security自动处理，名称默认：`username`、`password`，可配置

```java
.loginPage("/login")
```

# 9. 启用HTTP Basic认证👍

HTTP协议内容，与Spring框架无关。由于用户 ID 与密码是是以明文的形式在网络中进行传输的（尽管采用了 base64 编码，但是 base64 算法是可逆的），所以基本验证方案并**不安全**。

- 启用HTTP basic认证: `httpBasic()`
	- 默认关闭
- 在请求时带上用户名密码
	- `Authorization`属性
	- `https://username:password@www.example.com/`

[HTTP authentication - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication)

# 10. CSRF

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

- 解决：C每次提交表单A，**\_csrf 字段**有唯一ID，无法伪造
	- get得到`_csrf`， post请求携带`_csrf` ，防止第三方伪造

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F10%2F16%2F6a1a8a2f1ac8edf6f33638931d9d15b6_20231016210157.png)

# 11. CORS：跨域资源共享

- 是由浏览器定义的协议
	- 是一种基于 [HTTP](https://developer.mozilla.org/zh-CN/docs/Glossary/HTTP) 头的机制，该机制通过允许服务器标示除了它自己以外的其他[源](https://developer.mozilla.org/zh-CN/docs/Glossary/Origin)（域、协议或端口），使得浏览器允许这些源访问加载自己的资源。跨源资源共享还通过一种机制来检查服务器是否会允许要发送的真实请求，该机制通过浏览器发起一个到服务器托管的跨源资源的“预检”请求。在预检中，浏览器发送的头中标示有 HTTP 方法和真实请求中会用到的头。
- 常用于客户端与服务端分离的场景
	- 例：A提供静态页面，B提供Rest接口

# 12. 在后端代码获得用户信息

1. 注入`Principal`对象
	- 来自`java.security`，是JDK中JASS的低层框架
	- `String username = principal.getName()`获取用户名
2. `@AuthenticationPrincipal`注解
	- 来自`Spring Security`
	- `@AuthenticationPrincipal User user`作为函数参数获得user对象
3. 安全上下文获取

```java

Authentication authentication = SecurityContextHolder.getContext().getAuthentication(); User user = (User) authentication.getPrincipal();
```