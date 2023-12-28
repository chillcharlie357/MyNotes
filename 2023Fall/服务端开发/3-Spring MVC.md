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
date:  Tuesday,October 10th 2023
modified:  Thursday,December 28th 2023
---

# 1. 基本概念

**M**odel-**V**iew-**C**ontroller

- 模型model：存储内容，指数据、领域类
- 控制器controller：处理用户输入
- 视图view：显示内容

`@Slf4j`是lombok提供的  
`slf4j`是一个日志标准，具体实现有很多

# 2. Model 领域类

 `lombok`插件`@Data`注解

- `@ModelAttribute`
- `@SessionAttributes`
	- Model属性会复制到Servlet Request属性中，这样视图中就可以使用它们用于渲染页面

## 2.1. Servlet规范

- **Web容器的实现规范**，与Spring无关
	- Web容器/服务器，里面放Serlet对象
- 实现
	- tomcat
	- jetty

thymeleaf与Servlet request属性协作，<font color="#ff0000">与spring model解耦</font>

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

## 3.1. 处理过程

![Screenshot from 2023-10-07 18-38-55](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F18%2F10-13-42-ca4167c94583a1706c06fdd1405e61c3-Screenshotundefinedfromundefined2023-10-07undefined18-38-55-eb0c99.png)

## 3.2. MVC请求映射

- `Requestmapping`
- `GetMapping`
- `PostMapping`
- `DeleteMapping`
- `PatchMapping`
- `PutMapping`

## 3.3. 重定向

- http状态码：302
- 控制器`return redirect:<url>`

## 3.4. Spring MVC获取参数的集中方式

1. 表单(form)参数，转成model
	- 可能要自己实现Converter
	- 可以用`@Valid`校验
2. 路径参数
	- `@PathVariable`
	- `/book/{id}`
3. 请求参数/查询参数
	- `@RequestParam`
	- `challenge?model=2`
4. json请求体
	- `@RequestBody`，会用到HttpMessageConverter消息转换器
	- Rest API

