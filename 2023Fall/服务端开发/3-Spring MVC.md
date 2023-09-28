# 基本概念

**M**odel-**V**iew-**C**ontroller

- 模型model：存储内容，指数据、领域类
- 控制器controller：处理用户输入
- 视图view：显示内容

# 领域类

 `lombok`插件`@Data`注解

# MVC请求映射

`Requestmapping`
`GetMapping`
`PostMapping`
`DeleteMapping`
`PatchMapping`
`PutMapping`

# Servlet规范

- **Web容器的实现规范**，与Spring无关
	- Web容器/服务器，里面放Serlet对象
- 实现
	- tomcat
	- jetty

## Servlet对象

- 类型
	- Request
	- Response

Servlet对象带有属性(property即key-value)

## ModelAttribute

- `@ModelAttribute(name = <key>)`
	- 注解返回值为`value`
- 方法被Spring自动调用