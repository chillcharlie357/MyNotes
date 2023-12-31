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
title: 3-Spring MVC
date:  2023-10-10 18:10
modified:  2023-12-31 14:12
---

# 1. 基本概念👍

**M**odel-**V**iew-**C**ontroller

- 模型model：存储内容，指数据、领域类
- 控制器controller：处理用户输入，客户端的请求
- 视图view：显示内容

lombok在编译器自动生成一些样板代码，运行期没用。  
`@Slf4j`是lombok提供的注解。  
`slf4j`是一个日志标准，具体实现有很多，如logback,log4j等。  
`lombok`插件`@Data`注解

目前主流的是前后端分离开发。

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F31%2F11-21-23-58e755321af999ee54aaca9812997e0a-20231231112122-1cafd2.png)

# 2. Model 领域类

为视图提供数据。

- `@ModelAttribute`
- `@SessionAttributes`
	- Model属性会复制到**Servlet Request**属性中，这样视图中就可以使用它们用于渲染页面

## 2.1. Servlet规范

- 最小开发单元
- **Web容器的实现规范**，与Spring无关
	- Web容器/服务器，里面放Servlet对象
- 实现
	- tomcat
	- jetty

**thymeleaf**与**Servlet request**属性协作，<font color="#ff0000">与spring model解耦</font>

### 2.1.1. Servlet对象

- 类型
	- Request
	- Response
- Servlet对象带有属性(property即**key-value**)

## 2.2. ModelAttribute

- `@ModelAttribute(name = <key>)`
	- 注解返回值为`value`
- 方法被Spring自动调用

### 2.2.1. 方法级别

```java
@ModelAttribute("attributeName")
public SomeObject methodName() {
    // 创建并返回 SomeObject 对象
}
```

方法的返回值将被添加到模型中，并使用指定的属性名作为key。在控制器处理请求之前，该方法会先被调用。

### 2.2.2. 参数级别

```java
@GetMapping("/path")
public String methodName(@ModelAttribute("attributeName") SomeObject object) {
    // 处理请求
}
```

在这种写法中，`@ModelAttribute`注解标记在方法的参数上，指定了要从模型中获取的属性的名称。参数将被自动绑定到模型中的相应属性，以便在方法内部使用。

### 2.2.3. 默认命名规则

```java
@GetMapping("/path")
public String methodName(@ModelAttribute SomeObject object) {
    // 处理请求
}
```

如果没有指定`@ModelAttribute`注解的属性名称，Spring MVC会根据参数类型自动推断属性名称，并将其添加到模型中。  
在上面的示例中，`SomeObject`类型的参数将使用类名的首字母小写作为属性名称。

# 3. MVC请求

## 3.1. 处理过程👍

客户端请求在**后端的处理过程**，非常重要。  
**核心DispatcherServlet**，是Spring自己实现的Servlet容器。

1. Web容器开发的基本单元是Servlet，请求先到servlet。
2. mapping根据url把请求转到controller，其中spring框架会做参数解析。
3. controller拿到请求和请求参数，把请求和请求参数传给业务层
4. 业务层处理业务逻辑，可能会做数据持久化访问DAO层
5. 业务层把处理结构返回控制器
6. 控制器把结果放回给servlet
7. servlet拿到了数据和逻辑视图名，找到视图解析器的第三方库
8. 视图解析器渲染视图


![Screenshot from 2023-10-07 18-38-55](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F18%2F10-13-42-ca4167c94583a1706c06fdd1405e61c3-Screenshotundefinedfromundefined2023-10-07undefined18-38-55-eb0c99.png)

## 3.2. MVC请求映射

来自http协议的标准。  
注解可以放在类定义上方，也可以放在方法上方。

- `Requestmapping`
- `GetMapping`
- `PostMapping`
- `DeleteMapping`
- `PatchMapping`
- `PutMapping`

## 3.3. 重定向👍

控制器处理完成后可以返回逻辑视图名，也可以重定向到其他url

- http状态码：302
- 控制器`return redirect:<url>`

## 3.4. Spring MVC获取参数的集中方式👍

1. 表单(form)参数，转成model
	- 成员类型可能要自己实现Converter进行转换
	- 可以用`@Valid`校验
	- form是html里定义的
2. 路径参数
	- `@PathVariable`
	- 例：`/book/{id}`
3. 请求参数/查询参数
	- `@RequestParam`
	- 例：`/challenge?model=2`
4. json请求体
	- `@RequestBody`，会用到HttpMessageConverter消息转换器
	- Rest API

