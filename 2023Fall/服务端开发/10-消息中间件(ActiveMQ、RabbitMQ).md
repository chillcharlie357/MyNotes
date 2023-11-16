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
title: 10-消息中间件(ActiveMQ、RabbitMQ)
date:  Thursday,November 16th 2023
modified:  Thursday,November 16th 2023
---

# 消息中间件

- 提供消息服务的应用程序
- 主要用于组件之间的解耦，消息的发送者服务知道消息使用者的存在，反之依然

消息代理 broker：

![image.png](https://chillcharlie-img.oss-cn-hangzhou.aliyuncs.com/image%2F2023%2F11%2F16%2F18-53-01-0b10509f214aed7690a292bddcc3240f-20231116185259-10be9f.png)

# JMS

- Java Message Service
- JMS是一个Java标准，使用消息代理（message broker）的同意API
- JmsTemplate：Spring通过基于模板的抽象为JMS功能提供了支持

# ActiveMQ Artemis

- 支持协议
	- JMS
	- AMQP: Advanced Message Queueing Protocol
	- MQTT: Message Queuing Telemetry Transport
- 支持Native内存模式和JVM内存模式
	- Native可以绕过JVM，加快访问
- 消息持久化

## 直接使用JMS接口发送与接受消息

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

## 关键概念

- Message：类似广播, 生产端
- Destination：队列或主题。消费端

## JmsTemplate

- JmsTemplate是Spring对JMS集成支持的核心
- 发送的两个方法：send、convertAndSend

## 消息转换器 MessageConverter

MessageConverter是一个Spring的接口  
实现各种序列化机制

