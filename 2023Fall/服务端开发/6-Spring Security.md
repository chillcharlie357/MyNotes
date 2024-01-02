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
date:  2023-10-16 18:10
modified:  2024-01-02 16:01
---
- JAAS: Java Authentication Authorization Service. JDK提供的认证授权服务

# 1. Spring Security👍

- 划分为两类：
	1. 针对客户web请求权限控制
	2. 针对方法级的权限控制
		- 针对业务层代码
		- 调用前控制，调用后控制
		- 例：对数据库delete操作做权限控制

- 在spring中使用：添加依赖`spring-boot-starter-security`后会自动加载安全相关的bean

# 2. Cookie

维持客户端与服务端长时间的会话。

- http是无状态的协议，cookie可以保存状态
	- 例：保持用户登录状态

用户登录后，服务端会返回session-id，作为cookie存在本地每次请求都带上cookie

# 3. Web请求拦截

- spring security权限认证的原理：使用servlet容器的filter

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F10%2F26%2F092f577948a6053f598961fe1a329feb_20231026184400.png)

# 4. 开发人员还要做什么👍

<font color="#ff0000">除了框架提供的，开发人员还需要做什么，很重要</font>

1. 实现接口
	- `UserDetailsService`接口：给Spring框架提供用户详细信息。用户信息注册存储，需要用到用户信息的时候从数据访问层获取
		- 这里用到之前讲到数据访问层实现技术。
		- 和spring security解耦，只需要提供用户信息但不关心怎么实现。
	- 被Spring Security调用
2. 实现密码加密/解密对象
	- PasswordEncoder
	- Bean对象
3. (optional)实现登录页面
	- 有默认页面
	- `/login`, Spring已经自动实现了对应的`Controller`
4. 权限设定
	1. `SecurityFilterChain`，基于注入的`httpSecurity`对象
	2. 继承父类`WebSecurityConfigurerAdapter`，实现`configure`方法

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F10%2F26%2F4875ccdece8ae8892f54cde6042a6fef_20231026184427.png)

- PasswordEncoder：<font color="#ff0000">密码不能明文存储</font>，需要加密后再存到数据库里
	- 需要定义`Bean`

# 5. 框架实现了什么

1. 实现用户登录控制器get post
2. 请求重定向到用户登录页面
	- eg.用户未登录时，访问URL，服务端重定向到登录页面
3. 通过Filter对用户设定的权限进行权限控制

# 6. 用户信息存储👍

来自多个渠道，spring security不关心。

1. 内存用户数据库
2. JDBC用户存储
	- 不止是JDBC、关系型数据库，只要是持久化的都算
3. LDAP用户数据库
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
formLogin()
.loginPage("/login").usernameParamrter('username').passwordParameter('password')
```

# 9. 启用HTTP Basic认证👍

HTTP协议内容，与Spring框架无关。  
由于用户 ID 与密码是是以明文的形式在网络中进行传输的（尽管采用了 base64 编码，但是 base64 算法是可逆的），所以基本验证方案并**不安全**。

- 启用HTTP basic认证: `httpBasic()`
	- 默认关闭
- 在请求时带上用户名密码，一般在测试的时候使用
	- `Authorization`属性
	- `https://username:password@www.example.com/`

[HTTP authentication - HTTP | MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication)

# 10. CSRF

- 跨站请求伪造
	- 攻击者诱导受害者进入第三方网站，在第三方网站中，向被攻击网站发送跨站请求。利用受害者在被攻击网站已经获取的注册凭证，绕过后台的用户验证，达到冒充用户对被攻击的网站执行某项操作的目的。
- 默认开启
- 如何关闭：`.csrd().disable()`
- 如何忽略：`.ignoringAntMatchers("/admin/**")`

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F10%2F16%2F485966b40e6bb369ce1b793d15d48bf1_20231016205535.png)

- 例子：
	1. 受害者登录a.com，并保留了登录凭证（Cookie）。
	2. 攻击者引诱受害者访问了b.com。
	3. b.com 向 a.com 发送了一个请求：a.com/act=xx。浏览器会默认携带a.com的Cookie。
	4. a.com接收到请求后，对请求进行验证，并确认是受害者的凭证，误以为是受害者自己发送的请求。
	5. a.com以受害者的名义执行了act=xx。
	6. 攻击完成，攻击者在受害者不知情的情况下，冒充受害者，让a.com执行了自己定义的操作。

- 解决：C每次提交表单A，**\_csrf 字段**有唯一ID，无法伪造
	- get得到`_csrf`， post请求携带`_csrf` ，防止第三方伪造
	- 不在cookie中

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F10%2F16%2F6a1a8a2f1ac8edf6f33638931d9d15b6_20231016210157.png)

[前端安全系列（二）：如何防止CSRF攻击？ - 美团技术团队](https://tech.meituan.com/2018/10/11/fe-security-csrf.html)

# 11. CORS：跨域资源共享

- 是由浏览器定义的协议
	- 是一种基于 [HTTP](https://developer.mozilla.org/zh-CN/docs/Glossary/HTTP) 头的机制，该机制通过允许服务器标示除了它自己以外的其他[源](https://developer.mozilla.org/zh-CN/docs/Glossary/Origin)（域、协议或端口），使得浏览器允许这些源访问加载自己的资源。跨源资源共享还通过一种机制来检查服务器是否会允许要发送的真实请求，该机制通过浏览器发起一个到服务器托管的跨源资源的“预检”请求。在预检中，浏览器发送的头中标示有 HTTP 方法和真实请求中会用到的头。
- 常用于客户端与服务端分离的场景
	- 例：A提供静态页面，B提供Rest接口

# 12. 在后端代码获得当前登录的用户信息

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