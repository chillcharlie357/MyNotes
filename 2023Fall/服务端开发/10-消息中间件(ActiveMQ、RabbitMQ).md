
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

## 消息转换器

