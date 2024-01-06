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
title: 10-消息中间件(ActiveMQ、RabbitMQ)
date:  2023-11-16 18:11
modified:  2024-01-06 10:01
---

# 1. 消息中间件

- 提供消息服务的应用程序
- 主要用于组件之间的解耦，消息的发送者服务知道消息使用者的存在，反之依然

# 2. JMS👍

- Java Message Service
	- Jms规定了ConnectionFactory、Connection、Session等接口/类
- JMS是一个Java标准，使用消息代理（message broker）的同意API
- JmsTemplate：Spring通过基于模板的抽象为JMS功能提供了支持

消息代理 broker：

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F16%2F18-53-01-0b10509f214aed7690a292bddcc3240f-20231116185259-10be9f.png)

# 3. ActiveMQ Artemis

- 支持协议
	- JMS
	- AMQP: Advanced Message Queueing Protocol
	- MQTT: Message Queuing Telemetry Transport
- 支持Native内存模式和JVM内存模式
	- Native可以绕过JVM，加快访问
- 消息持久化

## 3.1. 直接使用JMS接口发送与接受消息

1. JMS规范：jakarta.jms-api-2.0.3.jar
2. atemis客户端：artemis-jms-client-2.17.0.jar
	- 与Spring无关

```java
ConnectionFactory connectionFactory = new ActiveMQConnectionFactory(BROKER_URL, USERNAME, PASSWORD);
Connection connection = connectionFactory.createConnection();
connection.start();
Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
Destination destination = session.createQueue("queue.example");
```

## 3.2. 关键概念👍

- Message：类似广播, 生产端
- Destination：队列或主题。消费端
	- 三种指定方式：
		1. application.yml（default-destination）
		2. @Bean（Destination对象）
		3. 直接String指定

## 3.3. JmsTemplate

- JmsTemplate是Spring对JMS集成支持的核心
- 发送的两个方法：send、convertAndSend

## 3.4. 消息转换器 MessageConverter👍

MessageConverter是一个Spring的接口，实现各种**序列化**机制。

- SimpleMessageConverter：实现String与TextMessage的相互转换、字节数组与BytesMessage的相互转换、Map与MapMessage的相互转换，以及Serializable对象与ObjectMessage的相互转换
- MappingJackson2MessageConverter：使用**Jackson 2 JSON**库实现消息与JSON格式的相互转换
- typeId：目的是告诉对方是什么类型，以便于反序列化
	- 字符串
- Message.setStringProperty，随属性集传输
- 消息转换器使用@Configuration定义成Bean

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F12%2F31%2F17-02-28-148a7c245eb17542de98c2c91efe9929-20231231170228-794f8b.png)

## 3.5. 接收模式

**拉取和推送都需要消息转换器做反序列化。**

### 3.5.1. 拉取模式 pull model

JmsTemplate支持  
访问URL

### 3.5.2. 推送模式 push model

需要定义消息监听器  
@JmsListener

# 4. RabbitMQ

**实现AMQP协议**。  
RabbitMQ基础概念详细介绍：[RabbitMQ基础概念详细介绍 - 割肉机 - 博客园](https://www.cnblogs.com/williamjie/p/9481774.html)  
也需要消息转换器，虽然包路径不一样，但是功能一样。

## 4.1. 概念👍

- ConnectionFactory、Connection、Channel
- Exchange：
	- Default、Direct、Topic、Fanout、Headers、Dead letter
- Queue
- Routing key：exchange根据routing key确定消息发往哪个队列
	- **JMS里没有这个概念**
- Binding key

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F16%2F20-31-55-d57a847ec3e2d2ed48f59f6b49ec2e0c-20231116203155-0bdbb4.png)

## 4.2. 使用

1. 添加依赖
2. 配置文件
3. 需要到控制台创建exchange，queue，binding

```yaml
spring:
rabbitmq:
host: localhost
port: 5672
username: guest
password: guest
template:
exchange: tacocloud.order
```

生产放指定exchange、routing key  
消费方只需关注队列名字

也有拉取模式和推送模式，也需要消息的转换