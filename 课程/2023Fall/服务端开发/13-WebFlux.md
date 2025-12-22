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
title: 13-WebFlux
date:  2023-12-07 18:12
modified:  2024-01-06 11:01
---

# 1. 异步Web框架的事件轮询机制

- 事件轮询：用较少的线程处理更多请求，减少线程管理的开销
	- 很多请求的线程并不是在运行，而是在等待
	- **事件驱动**
- 步骤
	1. 执行同步任务：所有同步任务都在主线程上执行，形成一个执行栈。
	2. 任务队列：主线程之外，还存在一个"任务队列"。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。
	3. 读取任务队列：一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。
	4. 事件循环：主线程不断重复上面的第三步，这样的一个循环称为事件循环

反应式编程适合**请求多**且**大部分在等待**的情况。事件轮询机制允许异步Web框架在等待某些操作（如网络请求或I/O操作）完成时，继续执行其他任务，从而提高了程序的效率和响应速度。

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F07%2F18-52-05-3d823dafa4673e8da18904eae9cbd7f0-20231207185204-00c0c0.png)

# 2. Spring MVC与Spring WebFlux

- 不同
	- MVC：依赖多线程处理，基于servlet api实现
		- 来一个请求就开一个线程
	- WebFlux：在事件轮询中处理请求
		- 可以使用纯粹的函数式编程实现Controller
- 共性
	- WebFlux也可以使用Controller、RequestMapping等注解
		- WebFlux的参数和返回值可能是流

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F07%2F19-00-35-8e48541635a29f83c6e89b87c0f1ee0d-20231207190034-169096.png)

依赖：spring-boot-starter-webflux

# 3. 实现

## 3.1. repository

- 继承`ReactiveCRUDrepository`接口
- 成员方法返回流

# 4. controller

- 返回流
- 如果方法**没有返回值**，需要在方法内**订阅**
	- 否则不会执行，不订阅就不会驱动
	- 多层嵌套流，对外围订阅不会触发内层的流

例子：

```java
    @GetMapping(value="/mono")
    public Mono<String> stringMono() {
        Mono<String> from = Mono.fromSupplier(() -> {
            try {
                TimeUnit.SECONDS.sleep(5);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            System.out.println(" in Supplier thread: " + Thread.currentThread().getName());
            return "Hello, Spring Reactive data time:" + LocalDateTime.now();
        });
        System.out.println("thread: " + Thread.currentThread().getName() + ", time:" + LocalDateTime.now());
        return from;
    }
```

```shell
Hello, Spring Reactive data time:2024-01-02T20:40:46.651302900
```

```java
    @GetMapping(value="/flux", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public Flux<String> produceFlux(){
        Flux<String> stringFlux = Flux.fromStream(IntStream.range(1,6).mapToObj(i->{
            try {
                TimeUnit.SECONDS.sleep(5);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            System.out.println(" in Supplier thread: " + Thread.currentThread().getName());
            return "java north flux："+i+", date time: "+LocalDateTime.now();
        }));
        System.out.println("thread: " + Thread.currentThread().getName() + ", time:" + LocalDateTime.now());
        return stringFlux;
    }

```

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F01%2F02%2F20-47-01-c9692a4a5af3f51e8de2b1c113c85c6c-20240102204701-f3eb3e.png)  
![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2024%2F01%2F02%2F20-47-19-a2f875c2613b77f4a8fe127cfb4a0331-20240102204719-e660d9.png)

## 4.1. 使用函数式范式定义控制器

- 基本组件
	- HandlerFunction：
	- RouterFunction：路由和处理关系

WebClient：相当于RestTemplate

# 5. R2DBC

反应式关系型数据库链接  
JDBC的替代方案
